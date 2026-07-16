# Glossary

Terms used throughout this portfolio — from API documentation, knowledge
bases, ITIL processes, and Russian/CIS documentation standards.

## API documentation

**API (Application Programming Interface)**
: A programmatic interface through which one system talks to another. In
a documentation context, it's the thing being documented: endpoints,
parameters, response formats.

**OpenAPI (formerly Swagger)**
: A formal specification for describing REST APIs in YAML/JSON. Acts as a
single source of truth from which documentation, client SDKs, and tests
can all be generated. See the example in this portfolio:
[`openapi.yaml` for the TaskFlow API](../api-docs/generated-reference/openapi.yaml).

**Docs-as-code**
: An approach where documentation lives in the same repository as the
code, is written in a plain-text format (Markdown, reStructuredText), and
goes through the same review process (pull requests) as code.

**Idempotency**
: A property of an operation where repeating it with the same parameters
doesn't change the result. Critical for handling network retries in an
API — see [PayFlow API "Authentication"](../api-docs/payment-api/authentication.en.md).

**Webhook**
: A mechanism where a server sends an HTTP request to a pre-registered URL
whenever an event occurs — an asynchronous alternative to polling.

**Rate limit**
: A cap on how many requests an API accepts per unit of time, usually
returned as a `429 Too Many Requests` error with a `Retry-After` header.

## Knowledge base and support

**KB (Knowledge Base)**
: A set of self-service articles for users and support agents, usually
following the pattern "symptom → steps → outcome."

**Troubleshooting article**
: A knowledge base article built around a specific symptom or error
message, rather than around a "do X from scratch" task.

**SSO (Single Sign-On)**
: An authentication mechanism where a user signs in once through a
corporate identity provider (Okta, Azure AD, etc.) and gets access to
multiple systems at once.

## Problem and incident management (ITIL)

**Incident**
: An unplanned interruption to, or reduction in the quality of, a
service. Response horizon: minutes to hours.

**Problem**
: The root cause of one or more incidents. Work horizon: days to weeks,
after the incident is already closed.

**KEDB (Known Error Database)**
: A registry of already-diagnosed issues with a confirmed workaround that
can be applied immediately.

**RCA (Root Cause Analysis)**
: A structured incident review whose goal is finding the root cause, not
just recording the symptom. See the
[Postmortem template](../problem-management/postmortem-template.en.md).

**Blameless postmortem**
: An incident review format that focuses on the conditions and systemic
factors that led to an error, rather than the person who made it.

**IC (Incident Commander)**
: The single person making decisions during an incident; coordinates the
response rather than personally fixing the system.

**SEV (Severity level)**
: A classification of an incident by scope of impact (e.g., SEV1 — the
service is down for all users). Determines who gets brought in — see the
[Escalation matrix](../problem-management/escalation-matrix.en.md).

**Runbook / playbook**
: A step-by-step guide for a standard operational situation, written so
it can be followed under time pressure without reading the whole document.

## Documentation standards (Russia/CIS)

**GOST 19 (ESPD)**
: The Unified System for Program Documentation. Largely retains 1970s–1980s
document numbering and structure, and is advisory rather than mandatory.

**GOST 34**
: A set of standards for automated systems, substantially updated in
2020–2021, and a good fit for modern IT system implementation projects.

**ESKD (Unified System for Design Documentation)**
: A set of standards for engineering/design documentation — used as a
reference where documentation concerns technical/engineering products
rather than just software.
