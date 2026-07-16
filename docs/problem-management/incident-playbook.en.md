# Incident response playbook

**Audience:** the on-call engineer who just received a TaskFlow alert.

!!! danger "How to use this document during a real incident"
    Don't read the whole document. Work through the steps in order and
    stop at the first one that applies to you. The "Further
    investigation" and "After the incident" sections aren't needed in the
    first 15 minutes — don't spend time on them now.

## Step 1 (0–2 min). Confirm the incident

- Open the **TaskFlow / Overview** dashboard in the monitoring system.
- Check: does the alert reflect real degradation (rising error rate,
  dropping availability), or is it a one-off false spike?
- If it's a false positive, `acknowledge` the alert with a comment and
  you're done with this document.
- If it's real, move to step 2.

## Step 2 (2–5 min). Determine severity

| Level | Criteria | Example |
|---|---|---|
| **SEV1** | The service is down for all users | The API returns 5xx on every request |
| **SEV2** | The service is degraded for a meaningful share of users | Elevated latency, errors in some regions |
| **SEV3** | Degradation affects a single feature or a small share of traffic | One rarely-used endpoint is broken |

Severity determines who gets pulled in — see the
[Escalation matrix](escalation-matrix.en.md).

## Step 3 (5–10 min). Open an incident channel

- Create a `#incident-YYYY-MM-DD-N` channel in your team chat.
- Assign an **Incident Commander (IC)** — by default this is the on-call
  engineer, unless a more senior engineer has already joined per the
  escalation matrix.
- Post an initial status update using this template:

```text
[SEV{X}] TaskFlow: {short description}
Affected: {what exactly is broken}
Status: investigating
IC: {name}
Next update: in 15 minutes
```

## Step 4 (10–15 min). Stabilize the service

The priority is **restoring service**, not finding the root cause.
Common quick actions, in increasing order of invasiveness:

1. Roll back the latest deploy, if the incident started within an hour
   of a release.
2. Flip a feature flag to disable the problematic functionality, if one
   exists.
3. Manually scale the service, if the degradation looks like overload.
4. Only if none of the above applies, move on to deeper diagnostics.

!!! warning "Don't confuse stabilizing with fixing"
    Rolling back a deploy, shedding load, or disabling a feature stops the
    bleeding — it doesn't find the cause. The actual root-cause analysis
    happens in the postmortem after the incident is closed, not now.

## Further investigation

If the standard quick actions didn't help, escalate to the next level per
the [Escalation matrix](escalation-matrix.en.md) and check the
[Known Error Database](known-error-database.en.md) — the symptom may have
occurred before, with a documented workaround.

## After the incident

- Update the incident channel status to `Status: resolved`.
- If it was SEV1 or SEV2, assign a postmortem owner and schedule the
  review within 3 business days, using the
  [Postmortem template](postmortem-template.en.md).
- If a temporary workaround was found during the incident, log it in the
  [Known Error Database](known-error-database.en.md) even before the
  postmortem happens.
