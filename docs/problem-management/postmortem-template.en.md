# Postmortem template (RCA)

**When to use it:** for any SEV1 or SEV2 incident, within 3 business days
of resolution. For SEV3, at the team's discretion.

!!! note "Blameless is a method, not a formality"
    A blameless postmortem looks for the **conditions** that made an
    error possible (insufficient monitoring, a confusing interface,
    missing guardrails) — not the **person** who made it. "The engineer
    rolled back the wrong service" gets rewritten as "the rollback
    interface didn't clearly show which service would be affected,
    leading to the wrong one being selected." This isn't softening the
    wording out of politeness — it's shifting focus to what can actually
    be fixed. You can scold a person once; you can fix a system for good.

## 1. Summary

Three or four sentences a person who hasn't read the rest of the document
can understand: what broke, how many users it affected, for how long, and
how it ended.

## 2. Impact

| Field | Value |
|---|---|
| Severity level | SEV{X} |
| Duration | from {HH:MM} to {HH:MM} ({N} minutes) |
| Users affected | {estimate: % or count} |
| Affected functionality | {e.g. creating tasks via the API} |
| Financial impact | {if applicable and measurable} |

## 3. Timeline

Only recorded facts with precise timestamps — no interpretation or
analysis in this section; that belongs under "Root cause."

```text
14:02 — Deployed version 1.4.2 to production
14:07 — Alert: /tasks/bulk error rate exceeded 5%
14:09 — On-call engineer acknowledged the incident, opened #incident-2026-07-14-1
14:11 — Declared SEV2, IC assigned
14:18 — Decision made to roll back deploy 1.4.2
14:24 — Rollback complete, error rate back to normal
14:30 — Incident closed, status "resolved"
```

## 4. Root cause

What went wrong and why — use the "5 whys" technique so you don't stop
at the first plausible explanation:

1. Why did `/tasks/bulk` requests return 504? — Because the database
   transaction didn't fit within the proxy timeout for batches > 500
   items.
2. Why were batches > 500 items allowed through? — Because batch-size
   validation lived on the client, not the server.
3. Why didn't the batch-size check catch this case? — Because version
   1.4.2 broke client-side validation as a side effect of an unrelated
   refactor.
4. Why didn't a pre-release test catch this? — Because the test suite
   had no case for a batch exactly at the boundary (500+1 items).
5. Why wasn't the boundary case in the tests? — Because the review
   checklist for validation changes doesn't explicitly require checking
   boundary values.

**Root cause:** missing server-side batch-size validation combined with a
gap in the review checklist, which meant the boundary case was never
tested.

## 5. What went well

Even in an incident, it's worth recording what worked correctly — this
guards against the wrong takeaway that "everything was bad," and shows
what shouldn't change.

- The alert fired 5 minutes after the degradation started — within the
  detection-time SLO (10 minutes).
- The rollback took 6 minutes thanks to a ready-made runbook.

## 6. What got in the way (what should improve)

- Batch-size validation existed on the client only — a single point of
  failure.
- The review checklist had no item about boundary values.

## 7. Action items

| Action | Owner | Due | Priority |
|---|---|---|---|
| Add server-side batch-size validation to `/tasks/bulk` | {name} | {date} | P1 |
| Add a boundary-value test case (500+1) to CI | {name} | {date} | P1 |
| Update the review checklist: explicit boundary-value item | {name} | {date} | P2 |
| File a [KEDB](known-error-database.en.md) entry if the permanent fix takes > 1 week | {name} | immediately | P1 |

!!! tip "Why every action item has an owner and a due date"
    An action item without an owner and a due date is a wish, not a plan.
    Postmortems whose action items routinely never close quickly lose the
    team's trust — people stop investing time in the analysis once they
    know nobody will act on the conclusions.

## 8. Links

- Incident channel: `#incident-{date}-{number}`
- Related KEDB entries: {links}
- Fix pull request: {link}
