# Errors

PayFlow returns errors in a single consistent shape, regardless of the
endpoint. This page is the single source of truth for error codes — every
other doc page links here instead of duplicating the tables.

## Error shape

```json
{
  "error": {
    "type": "card_error",
    "code": "card_declined",
    "message": "The card was declined by the issuer.",
    "decline_code": "insufficient_funds",
    "request_id": "req_9F3kLp2Q"
  }
}
```

| Field | Description |
|---|---|
| `type` | Error category (see table below) — use this for programmatic handling |
| `code` | Specific error code — for finer-grained handling and logging |
| `message` | Human-readable description — **don't parse this in code**, it can change |
| `decline_code` | `card_error` only: the reason the card issuer gave for declining |
| `request_id` | Request identifier — include it when contacting PayFlow support |

!!! tip "Why `message` isn't for parsing"
    We can change and translate `message` text without notice — it's part
    of the UX, not the API contract. The contract is `type` and `code`.
    This caveat is the thing most often left out of payment API docs,
    which then breaks integrations the moment wording changes.

## HTTP statuses

| Status | Meaning |
|---|---|
| `200 OK` | The request succeeded |
| `400 Bad Request` | Malformed request parameters |
| `401 Unauthorized` | Missing or invalid API key |
| `402 Payment Required` | The request was well-formed but the payment was declined (see `card_error`) |
| `404 Not Found` | No object with that ID exists in this mode (test/live) |
| `409 Conflict` | `Idempotency-Key` conflicts with other request parameters |
| `429 Too Many Requests` | Rate limit exceeded — see below |
| `500 / 503` | Error on PayFlow's side — retry with exponential backoff |

## Error types (`type`)

| `type` | When it happens | What to do |
|---|---|---|
| `card_error` | The card was declined by the issuer or bank | Show the user a clear reason, offer another card |
| `validation_error` | A required parameter is missing or malformed | Integration-side bug — check the request against the [parameter reference](../generated-reference/index.en.md) |
| `authentication_error` | Invalid key, or a key from the wrong mode (test/live) | Confirm the key exists and matches the intended mode |
| `idempotency_error` | Same `Idempotency-Key` but different request parameters | Use a new idempotency key for the new request |
| `rate_limit_error` | Too many requests per second | See "Rate limits" below |
| `api_error` | An internal PayFlow error | Retry the request; if it persists, contact support with the `request_id` |

## Common `decline_code` values for `card_error`

| `decline_code` | Typical cause | Recommendation for the user |
|---|---|---|
| `insufficient_funds` | Not enough balance on the account | Offer another payment method |
| `card_declined` | A generic bank decline with no further detail | Suggest another card, or contact the bank |
| `expired_card` | The card has expired | Ask the user to update their card details |
| `incorrect_cvc` | Wrong CVC code | Ask the user to re-enter their card details |
| `processing_error` | A temporary error on the card network's side | Suggest trying again shortly |

## Rate limits

By default: **100 requests per second** per account in live mode, **25
requests per second** in test mode. When exceeded, PayFlow returns `429`
with a `Retry-After` header (in seconds).

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 2
```

Recommended retry strategy: exponential backoff with jitter:

```python
import time
import random

def request_with_retry(fn, max_attempts=5):
    for attempt in range(max_attempts):
        response = fn()
        if response.status_code != 429:
            return response
        delay = min(2 ** attempt + random.random(), 30)
        time.sleep(delay)
    raise RuntimeError("Retry attempts exceeded")
```

## Next

- [Webhooks](webhooks.en.md) — an asynchronous alternative to polling for status
- [Knowledge base → 429 Too Many Requests](../../knowledge-base/kb-rate-limit-429.en.md) —
  a troubleshooting article for users who hit the rate limit
