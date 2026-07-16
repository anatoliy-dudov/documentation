# Fixing 429 Too Many Requests

**Who this is for:** developers integrating the TaskFlow API.

**Symptom:** API requests occasionally return `HTTP 429` instead of the
expected response, especially during bulk data syncs (e.g., an initial
task import).

## Why this happens

The TaskFlow API rate-limits requests at the workspace level: **120
requests per minute** on the standard plan. The limit is shared across all
keys in the same workspace — if several of your services use the same API
key, they share a single limit.

This isn't an integration bug by itself — it's expected behavior once the
limit is exceeded. The goal is to handle `429` correctly, not to avoid it
entirely.

## How to quickly confirm it's rate limiting

A `429` response includes a `Retry-After` header — the number of seconds
to wait before retrying:

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 8
Content-Type: application/json

{"error": {"code": "rate_limited", "message": "Rate limit exceeded"}}
```

If `Retry-After` is present and `error.code` is `rate_limited`, it's rate
limiting, not a different problem.

## Fix

1. **Implement exponential backoff with jitter.** Don't retry
   immediately — use `Retry-After` as the minimum delay, and add random
   jitter to avoid a "thundering herd" of retries from multiple workers
   at once.
2. **Limit concurrency during bulk syncs.** If you're importing thousands
   of tasks, use a queue with a bounded number of concurrent requests
   (e.g., 5 concurrent requests instead of 100).
3. **Cache where it makes sense.** If the same task list is requested
   multiple times in a short window (e.g., by different parts of your
   app), cache the response for a few seconds instead of re-querying the
   API.
4. **Use bulk endpoints for large volumes.** Wherever the TaskFlow API
   offers batch operations (`POST /tasks/bulk`), they count as a single
   request against the limit instead of N — use them instead of looping
   individual calls.

## If the standard plan's limit genuinely isn't enough

The limit can be raised on the Enterprise plan, or discussed individually
with the support team — contact support describing your use case
(expected requests per minute and traffic pattern: steady or bursty).

## Related articles

- [PayFlow API — errors and status codes](../api-docs/payment-api/errors.en.md) —
  a similar approach to handling `429` in another API in this portfolio,
  if you'd like to compare solutions.
- [FAQ](kb-faq.en.md)
