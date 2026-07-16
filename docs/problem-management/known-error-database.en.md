# Known Error Database (KEDB)

**What this is.** A list of already-diagnosed issues with a temporary
workaround that can be applied immediately, without waiting for a
permanent fix. This is not a list of open bug-tracker tickets — an entry
only gets added here once a working cause, or at least a reliable
workaround, has been found.

!!! tip "One symptom per entry"
    Each entry should cover a single, specific symptom. If one root cause
    produces two different observable symptoms, create two entries and
    link them via "Related to," rather than describing both symptoms in
    one entry. That way an entry is found by what the on-call engineer
    actually sees, not by an internal cause they don't know yet.

## KEDB-042: Elevated latency on `POST /tasks/bulk` for batches > 500 items

- **Symptom:** requests to `POST /tasks/bulk` with more than 500 tasks in
  a batch exceed the client timeout (30 seconds) and return `504`.
- **Root cause:** batch processing runs sequentially inside a single
  database transaction; above 500 items, the transaction no longer fits
  within the proxy server's timeout.
- **Workaround:** split the batch into chunks of no more than 500 tasks
  and send several sequential requests instead of one large one.
- **Permanent fix:** planned migration to asynchronous batch processing
  with a completion callback (see postmortem PM-2026-014).
- **Affected versions:** API v1.0–v1.4 (current).
- **Status:** known issue, workaround confirmed.

## KEDB-039: False `payment.failed` webhooks on acquirer bank timeout

- **Symptom:** PayFlow sends a `payment.failed` webhook, but a manual
  check via `GET /payments/{id}` shows the status as `succeeded`.
- **Root cause:** when the acquiring bank's response times out, the
  payment is briefly marked `failed`, then updated to `succeeded` once the
  bank's delayed response arrives — but the `payment.failed` event has
  already been sent to webhook subscribers.
- **Workaround:** before taking irreversible action on `payment.failed`
  (e.g., cancelling an order), re-check the current status via
  `GET /payments/{id}` instead of trusting the webhook event alone.
- **Permanent fix:** add a separate `payment.failed.tentative` webhook
  event for acquirer timeouts, reserving `payment.failed` for final
  declines only. In development.
- **Affected versions:** all API versions.
- **Status:** known issue, workaround confirmed.

## How to use this database during an incident

1. Search for an entry by the symptom you're observing (Ctrl+F on this
   page), not by a suspected cause.
2. If an entry is found, apply the workaround immediately — no root-cause
   investigation is needed.
3. If no entry exists, create a new one using this same template after
   the incident is closed, even if a permanent fix isn't ready yet.
