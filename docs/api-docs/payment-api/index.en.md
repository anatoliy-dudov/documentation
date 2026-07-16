# PayFlow API — annotated documentation

PayFlow is a fictional payment API documented by hand — unlike the
[generated TaskFlow reference](../generated-reference/index.en.md). The example
mirrors how documentation for popular payment providers is usually
organized: a quickstart, authentication, a single error reference, and
webhooks.

!!! note "Reviewer's note"
    This section intentionally keeps more "why" commentary than production
    docs normally would, so the reasoning stays visible. In a real project,
    much of this would live in an ADR (architecture decision record) or a
    pull request comment, and the shipped docs would read drier.

## Section structure

| Page | What it covers | Primary audience |
|---|---|---|
| [Quickstart](quickstart.en.md) | The path from sign-up to a first successful payment in 5 steps | A developer doing their first integration |
| [Authentication](authentication.en.md) | API keys, test/live mode, idempotency | A developer setting things up, a security reviewer |
| [Errors](errors.en.md) | Error shape, codes, what to do about them | A developer debugging an integration |
| [Webhooks](webhooks.en.md) | Asynchronous payment status notifications | A developer building event-handling backend logic |

## Principles behind this write-up

1. **Task → concept → reference.** The quickstart gives a step-by-step
   sequence. Authentication and webhooks explain concepts. Errors is a
   reference page you come back to for a specific lookup. These are
   different reading modes (`step-by-step` vs. `lookup`), and mixing them
   on one page hurts both.
2. **One source of truth for error codes.** Every error code is documented
   exactly once, on the [Errors](errors.en.md) page. Other pages link to it
   instead of duplicating the table — otherwise an API update means
   syncing several copies.
3. **Explicit test/live separation.** Payment APIs are a high-risk area: one
   missed sentence about live mode can lead to a real charge during
   testing. This is called out on more than one page rather than once at
   the very top, where it's easy to miss.
4. **Code samples in more than one language.** A technical writer doesn't
   need to maintain SDKs for ten languages, but should be able to show at
   least `curl` plus one server-side language (here, Python) so the
   example is copy-paste runnable.

## What this does not cover

Being upfront about the example's boundaries:

- This is not a real payment provider — every identifier, amount, key, and
  parameter is fictional.
- The section doesn't cover the full lifecycle (refunds, disputes,
  multi-currency, PCI DSS compliance) — that would multiply the volume
  without adding much for the portfolio's purpose.

---

Next: [Quickstart →](quickstart.en.md)
