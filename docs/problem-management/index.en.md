# Problem Management: section overview

The material in this section is written from the perspective of the
on-call team supporting **TaskFlow** itself (the same product used in the
API documentation and knowledge base sections) — this is internal
operational documentation, not customer-facing documentation.

## Incident management vs. problem management

In ITIL terms these are distinct, related processes — and documentation
should treat them differently:

- **Incident management** — "the service is broken right now, restore it
  as fast as possible." Time horizon: minutes to hours. See the
  [Incident response playbook](incident-playbook.en.md).
- **Problem management** — "why did this happen at all, and how do we make
  sure it doesn't happen again." Time horizon: days to weeks, after the
  incident is already closed. See the
  [Known Error Database](known-error-database.en.md) and the
  [Postmortem template](postmortem-template.en.md).

Documentation for these two processes needs to look and function
differently: during an incident, the reader has no time to page through a
long document, whereas a problem review needs to be thorough.

## Material in this section

- [Incident response playbook](incident-playbook.en.md) — what an on-call
  engineer does in the first 15 minutes of an incident.
- [Known Error Database (KEDB)](known-error-database.en.md) — already
  diagnosed issues with a temporary workaround, so they don't need to be
  re-investigated from scratch.
- [Postmortem (RCA) template](postmortem-template.en.md) — the structure
  for reviewing an incident after it's closed, blameless by design.
- [Escalation matrix](escalation-matrix.en.md) — who to bring in and when,
  based on incident severity.
