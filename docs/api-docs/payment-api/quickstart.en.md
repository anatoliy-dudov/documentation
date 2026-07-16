# Quickstart

Five steps to send your first test payment through the PayFlow API.

!!! warning "Test mode"
    Every request in this guide uses test keys (`pf_test_...`). In test
    mode no money moves — it's an isolated sandbox. More on modes on the
    [Authentication](authentication.en.md) page.

## Step 1. Create an account

Sign up at [dashboard.payflow.example](https://dashboard.payflow.example)
(a demo address). After sign-up, PayFlow automatically generates a pair
of test keys for you — publishable and secret.

## Step 2. Find your API keys

Under **Dashboard → Developers → API keys** you'll find four keys:

| Key | Prefix | Used where |
|---|---|---|
| Publishable test key | `pf_test_pk_...` | Frontend, in the browser |
| Secret test key | `pf_test_sk_...` | Backend, never in the browser |
| Publishable live key | `pf_live_pk_...` | Production frontend |
| Secret live key | `pf_live_sk_...` | Production backend |

This guide only needs the **secret test key**.

## Step 3. Send your first request

=== "curl"

    ```bash
    curl https://api.payflow.example/v1/payment_intents \
      -u pf_test_sk_51H8x...: \
      -d amount=2000 \
      -d currency=usd \
      -d "payment_method_types[]"=card
    ```

=== "Python"

    ```python
    import requests

    response = requests.post(
        "https://api.payflow.example/v1/payment_intents",
        auth=("pf_test_sk_51H8x...", ""),
        data={
            "amount": 2000,
            "currency": "usd",
            "payment_method_types[]": "card",
        },
    )
    print(response.json())
    ```

`amount` is expressed in the currency's smallest unit — so `2000` for USD
means **$20.00**, not $2000. This is a common source of integration bugs,
which is why it's called out in the very first example rather than buried
in a parameter reference.

A successful response:

```json
{
  "id": "pi_3N8x2ZKq...",
  "object": "payment_intent",
  "amount": 2000,
  "currency": "usd",
  "status": "requires_payment_method",
  "client_secret": "pi_3N8x2ZKq..._secret_..."
}
```

## Step 4. Confirm the payment with a test card

Use the test card number `4242 4242 4242 4242`, any future expiry date,
and any CVC. PayFlow recognizes the `4242...` range as a test card that
always succeeds.

| Card number | Scenario |
|---|---|
| `4242 4242 4242 4242` | Successful payment |
| `4000 0000 0000 0002` | Card declined (`card_declined`) |
| `4000 0000 0000 9995` | Insufficient funds (`insufficient_funds`) |

Full list of decline codes on the [Errors](errors.en.md) page.

## Step 5. Check the status

```bash
curl https://api.payflow.example/v1/payment_intents/pi_3N8x2ZKq... \
  -u pf_test_sk_51H8x...:
```

If `status` is `succeeded`, the payment went through. Instead of polling
the API manually, wire up [webhooks](webhooks.en.md): PayFlow will push a
`payment_intent.succeeded` event to you.

## Next

- [Authentication](authentication.en.md) — how keys and idempotency work
- [Webhooks](webhooks.en.md) — asynchronous status notifications
- [Errors](errors.en.md) — the full code reference
