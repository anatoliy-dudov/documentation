# Checklists

Working checklists I use to keep documentation consistent regardless of
who writes it or when.

## API documentation review checklist

Before a page or an OpenAPI spec change ships:

- [ ] Every endpoint has a `summary` (short) and a `description` (explains
      non-obvious behavior instead of restating the `summary` in other
      words).
- [ ] Destructive operations (`DELETE`, irreversible `POST`) explicitly
      state what happens to the data — permanently deleted vs. archived.
- [ ] Every parameter and request body field has a realistic `example`,
      not a placeholder like `string`/`123`.
- [ ] Error codes link to a single error reference instead of being
      re-described on every page.
- [ ] Fields with a fixed set of values are modeled as `enum`, not a free
      string with the options only listed in prose.
- [ ] A newly added required parameter is flagged as a breaking change in
      the changelog.
- [ ] Request examples were copied and actually executed at least once
      (curl or equivalent) — not just written from memory.
- [ ] The spec passes linting (e.g. Spectral) with no error-level issues.

## Knowledge base article publishing checklist

- [ ] The title is phrased as a real user search query ("429 error
      during task import"), not an internal feature name.
- [ ] The first paragraph answers "is this about my situation?" within
      3–5 seconds of reading.
- [ ] Steps are numbered and executable in order, with no hidden
      prerequisites mentioned only midway through the article.
- [ ] There's an "If this didn't help" section linking to a next step
      (escalate to support, a related article) rather than a dead end.
- [ ] The article states which plan/role/product version it applies to,
      if that limits applicability.
- [ ] The article has been checked against the current UI (screenshots,
      menu labels) within the last 2 release cycles.
- [ ] Related articles are explicit links, not a generic "see also the
      docs" phrase.

## Pre-incident-closure checklist (for the IC)

- [ ] The incident channel status is updated to "resolved" with an exact
      timestamp.
- [ ] For SEV1/SEV2 — a postmortem owner is assigned and a review date is
      scheduled (within 3 business days).
- [ ] Any temporary workaround used is recorded in the
      [KEDB](../problem-management/known-error-database.en.md), even if
      the postmortem hasn't happened yet.
- [ ] Affected users/stakeholders received a final "service restored"
      notice, if they received interim updates.
- [ ] Everyone brought in via escalation is explicitly notified the
      incident is closed.

## Release notes publishing checklist

- [ ] Changes are grouped by type: added / changed / fixed / deprecated.
- [ ] Breaking changes are called out in a separate block at the top,
      not mixed in with other items.
- [ ] Each item links to further detail (issue, doc page) if the entry is
      shorter than one full sentence.
- [ ] Wording is written from the user's point of view ("you can now do
      X"), not from an internal-implementation point of view ("refactored
      module Y").
- [ ] Version and release date appear in the heading in one consistent
      format across the whole release notes history.
