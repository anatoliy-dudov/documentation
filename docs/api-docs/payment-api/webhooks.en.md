# Webhooks

Polling the API for payment status changes works, but it's inefficient.
Webhooks solve the same problem asynchronously: PayFlow sends a POST
request to your endpoint whenever an event happens.

## Events PayFlow sends

| Event | When it fires |
|---|---|
| `payment_intent.succeeded` | The payment completed successfully |
| `payment_intent.payment_failed` | A payment attempt was declined |
| `charge.refunded` | A refund was issued |
| `payment_intent.requires_action` | Extra authentication is needed (e.g. 3-D Secure) |

## Setting up an endpoint

1. Create a publicly reachable HTTPS endpoint, e.g. `POST /webhooks/payflow`.
2. Register it under **Dashboard → Developers → Webhooks**, selecting the
   events you need.
3. PayFlow issues a **webhook signing secret** (`whsec_...`) — you'll need
   it to verify requests are authentic (see below).

Example request body:

```json
{
  "id": "evt_1N8y3Kq...",
  "type": "payment_intent.succeeded",
  "created": 1719840000,
  "data": {
    "object": {
      "id": "pi_3N8x2ZKq...",
      "amount": 2000,
      "currency": "usd",
      "status": "succeeded"
    }
  }
}
```

## Verifying the signature

!!! danger "Required, not optional"
    Anyone who learns your endpoint's URL can POST a fake body designed
    to look like a PayFlow event. Without signature verification, your
    server can't tell a genuine payment notification from a forged one —
    meaning an attacker could, for example, trigger order fulfillment
    without an actual payment.

PayFlow signs every request with HMAC-SHA256 and sends the signature in
the `PayFlow-Signature` header:

```python
import hashlib
import hmac

def verify_signature(payload: bytes, signature_header: str, secret: str) -> bool:
    expected = hmac.new(secret.encode(), payload, hashlib.sha256).hexdigest()
    return hmac.compare_digest(expected, signature_header)
```

```python
# Example Flask handler
@app.route("/webhooks/payflow", methods=["POST"])
def handle_webhook():
    payload = request.get_data()
    signature = request.headers.get("PayFlow-Signature", "")

    if not verify_signature(payload, signature, WEBHOOK_SECRET):
        return "Invalid signature", 400

    event = json.loads(payload)
    if event["type"] == "payment_intent.succeeded":
        mark_order_as_paid(event["data"]["object"]["id"])

    return "", 200
```

## Handling events idempotently on your side

PayFlow guarantees **at-least-once** delivery — meaning the same event
could theoretically arrive twice (for example, if your server returned
`200 OK` but the response got lost in transit). Handle events
idempotently: store the event `id` and check whether you've already
processed it before changing order state.

## Delivery retries

If your endpoint doesn't respond with `2xx` within 10 seconds, PayFlow
retries on a schedule: after 5 minutes, 30 minutes, 2 hours, 6 hours, then
every 12 hours — for up to 72 hours total. After that, the event is
marked undelivered and only available manually through the Dashboard.

## Testing locally

A local server isn't reachable from the internet directly, so use a
tunnel for testing webhooks (e.g. `payflow-cli listen`, similar to
`stripe listen`), which forwards events to `localhost`:

```bash
payflow-cli listen --forward-to localhost:4000/webhooks/payflow
```

## Next

- [Errors](errors.en.md) — what synchronous API errors mean
- [Knowledge base → PayFlow webhook not arriving](../../knowledge-base/kb-webhook-troubleshooting.en.md) —
  a troubleshooting article for common causes of undelivered events
