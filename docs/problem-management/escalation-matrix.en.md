# Escalation matrix

Answers one question: **who to bring in, and after how long**, if an
on-call engineer can't stabilize an incident alone. Used together with
the [Incident Response Playbook](incident-playbook.en.md).

!!! tip "Why a matrix, not a chat contact list"
    A contact list only answers "who do I message." A matrix additionally
    fixes **when** and **under what condition** — removing the need to
    decide that under stress, when decisions tend to be worse than in a
    calm setting.

## Roles

| Role | Responsibility | Usually who |
|---|---|---|
| **Incident Commander (IC)** | The single person making decisions about the incident; coordinates rather than fixes hands-on | The on-call engineer by default; a more senior engineer if one has joined |
| **Communications Lead** | Updates in the incident channel, stakeholder status, and (if needed) a public status page | Assigned by the IC on SEV1/SEV2; usually not needed separately for SEV3 |
| **Subject Matter Expert (SME)** | An expert on a specific system component, pulled in on the IC's request | The owner of the relevant service |

## When and who to escalate to

| Condition | Action | Max time before escalation |
|---|---|---|
| Incident confirmed, severity not yet determined | On-call engineer determines the SEV level per the [playbook](incident-playbook.en.md) | 5 minutes from confirmation |
| SEV1 declared | Automatic notification to the engineering lead and the SME for the affected component | Immediately |
| SEV2, standard quick actions haven't helped after 15 minutes | Bring in the SME for the affected component | 15 minutes |
| SEV1, not stabilized after 20 minutes | Bring in the engineering lead as a second IC candidate | 20 minutes |
| Any SEV, a public status is needed (external users clearly affected) | Bring in a Communications Lead | 15 minutes |
| SEV1, not stabilized after 45 minutes | Bring in the department head (Head of Engineering/CTO) | 45 minutes |

## Handing off the IC role

Changing the IC is normal practice during a long incident (e.g. an
on-call time-zone handoff), not a sign of failure. Handoff sequence:

1. The new IC explicitly confirms taking the role in the incident
   channel: `IC changing: {old name} → {new name}`.
2. The previous IC gives a brief status summary: what's already been
   checked, what's been ruled out, what's happening right now.
3. Until the new IC has explicitly confirmed, the old IC remains
   responsible — the role is never considered handed off "by default."

## Contacts by component

| Component | SME / service owner | Escalation channel |
|---|---|---|
| TaskFlow API (core) | Platform team | `@platform-oncall` |
| PayFlow (payments) | Payments team | `@payments-oncall` |
| Authentication / SSO | Identity team | `@identity-oncall` |
| Infrastructure / database | SRE team | `@sre-oncall` |

!!! warning "Keep this table current"
    A stale escalation matrix is worse than no matrix at all: it creates
    false confidence that the right person will be found automatically.
    The owner of the incident management process is responsible for
    keeping it updated — reviewed at least quarterly, and after any team
    reorganization.
