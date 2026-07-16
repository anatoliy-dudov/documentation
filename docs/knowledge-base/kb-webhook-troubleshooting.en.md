# PayFlow webhook isn't reaching my server

**Who this is for:** developers integrating PayFlow API webhooks.

**Symptom:** the Dashboard shows the event as sent, but your server
doesn't receive or process it.

## Step 1. Check the delivery log

Under **Dashboard → Developers → Webhooks → [your endpoint] → Logs** you
can see every delivery attempt along with your server's HTTP response
status. Start here rather than in your application logs — it immediately
tells you which side the problem is on.

| What the log shows | Likely cause |
|---|---|
| No attempts at all | The event isn't subscribed on this endpoint — check the event list in settings |
| Timeout (no response within 10 seconds) | Your handler does slow work synchronously (see step 2) |
| `400 Invalid signature` | Signature verification is failing (see step 3) |
| `500` from your server | A bug in the handler code — check application logs |
| `200`, but nothing updated | The event arrived, but the business logic inside the handler didn't fire |

## Step 2. The handler responds slower than 10 seconds

A common mistake is doing heavy synchronous work inside the webhook
handler (sending an email, generating a PDF, calling another slow API).
PayFlow counts a timeout as a failed delivery and retries, even if the
work eventually finished.

**Fix:** the handler should save the event as quickly as possible (e.g.
to a job queue) and return `200 OK` right away. Heavy logic runs
asynchronously, separate from the webhook response.

## Step 3. Signature verification fails

The most common causes of `400 Invalid signature`:

1. **The request body was rewritten by the framework's parser.** Some web
   frameworks parse JSON by default and don't expose the raw request
   body — but the signature is computed over the raw bytes. Make sure
   this endpoint reads `request.raw_body` instead of re-serializing the
   parsed JSON before verifying.
2. **A secret from the wrong mode (test/live) is being used.** Test and
   live webhooks have different signing secrets — check that the secret
   in your environment variables matches the mode the event was created
   in.
3. **The secret was rotated in the Dashboard** but not updated in the
   service's environment variables.

Details on the signing mechanism itself are on the
[PayFlow API "Webhooks" page](../api-docs/payment-api/webhooks.en.md).

## Step 4. The event was delivered, but the order status didn't update

If the HTTP response status is `200` but the order in your system didn't
change, the problem isn't delivery — it's the handler's logic. A common
cause: the code only updates order status for `payment_intent.succeeded`
but doesn't handle `payment_intent.payment_failed`, which makes "stuck"
orders look like a delivery bug even though the webhook fired correctly.

## If nothing above helps

Gather this before contacting support:

- the `request_id` from the Dashboard delivery log;
- the exact time the event should have arrived;
- the HTTP status your server returned (from the Dashboard log, not your
  own logs — they sometimes disagree).

## Related

- [PayFlow API docs → Webhooks](../api-docs/payment-api/webhooks.en.md)
- [PayFlow API errors and status codes](../api-docs/payment-api/errors.en.md)
