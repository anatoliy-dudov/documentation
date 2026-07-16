# Authentication

PayFlow uses API keys over HTTP Basic Auth: the key is sent as the
username, and the password is left blank.

```bash
curl https://api.payflow.example/v1/payment_intents \
  -u pf_test_sk_51H8x...:
```

## Test mode vs. live mode

Every account has two independent modes — **test** and **live** — each
with its own key pair (publishable + secret). Data never crosses between
modes: a payment created with a test key will never show up in a live
statement.

!!! danger "The most common integration mistake"
    Using `pf_live_sk_...` in code that runs in the browser (React, Vue, a
    plain `<script>` tag). The secret key is visible in devtools — any
    visitor to the page gets full access to your PayFlow account. The
    secret key lives **on the server only**.

| | Publishable key (`pk`) | Secret key (`sk`) |
|---|---|---|
| Where to store it | Client (browser, mobile app) | Server, secrets manager |
| What it allows | Creating a card token, initializing the payment form | Any operation: charges, refunds, reading data |
| Impact if leaked | Nothing critical — that's the point of a publishable key | Full account compromise — rotate immediately |

## Key rotation

The secret key needs to be rotated:

1. **Immediately** — if there's any suspicion of a leak (the key showed up
   in a public repo, logs, or a screenshot).
2. **On a schedule** — every 90–180 days, per your internal security
   policy.

Zero-downtime rotation sequence:

1. Create a new secret key in the Dashboard (the old one keeps working).
2. Roll the new key out to every service that talks to PayFlow.
3. Confirm traffic on the old key has stopped (Dashboard → API keys →
   Usage).
4. Revoke the old key.

## Idempotency

Network failures happen: a client sends a charge request but never gets
a response because of a timeout. Simply retrying the request risks
charging the customer twice. To avoid that, send an `Idempotency-Key`
header with a unique value per **logical** operation:

```bash
curl https://api.payflow.example/v1/payment_intents \
  -u pf_test_sk_51H8x...: \
  -H "Idempotency-Key: order-8842-attempt-1" \
  -d amount=2000 -d currency=usd
```

If a request with the same `Idempotency-Key` arrives again within 24
hours, PayFlow returns the result of the first request instead of
performing the operation again.

!!! tip "Why this lives here, not in the endpoint reference"
    Idempotency isn't a property of one endpoint — it's a cross-cutting
    concept for the whole API. Describing it only under
    `POST /payment_intents` parameters means a developer reading about
    `/refunds` might never learn the same mechanism applies there too.

## Next

- [Errors](errors.en.md) — what to do when a request fails
- [Webhooks](webhooks.en.md) — how to verify incoming notifications
