# API Documentation: section overview

This section shows two different approaches to documenting an API. They
solve different problems, and in real projects I usually use both at the
same time.

## 1. Auto-generated reference — TaskFlow API

A full endpoint reference generated from an **OpenAPI 3.0** specification.
This approach works well when:

- the API is large and changes quickly — maintaining a reference by hand
  would be too expensive;
- you need a single source of truth (the spec) for documentation, tests,
  and client SDKs;
- accuracy on every parameter, response code, and object schema matters
  more than narrative explanation.

In this approach, my role isn't to write the reference by hand, but to:

- design and review the OpenAPI spec itself (naming, descriptions,
  examples, error codes);
- set up spec linting and build tooling (e.g., Spectral);
- write the supporting material — conceptual pages a generator can't
  produce on its own.

[:octicons-arrow-right-24: See the example](generated-reference/index.en.md)

## 2. Annotated documentation — PayFlow API

Documentation for a fictional payment API in the style of popular payment
providers: quickstart, authentication, error codes, webhooks. Hand-written,
with explanations of the reasoning behind it — each section describes not
just *what* is documented, but *why* it's documented that way.

This approach is needed when:

- the API is small to medium-sized, and precise wording matters more than
  update speed;
- the audience is external developers integrating payments for the first
  time, who need a clear path from "got my API key" to "first payment
  succeeded";
- some content (concepts, edge cases, security guidance) simply can't be
  expressed in an OpenAPI spec alone.

[:octicons-arrow-right-24: See the example](payment-api/index.en.md)

## How to compare these two examples

| | TaskFlow API (generated) | PayFlow API (annotated) |
|---|---|---|
| Content source | OpenAPI specification | Hand-written Markdown |
| Strengths | Completeness, consistency, update speed | Explaining "why", handling edge cases |
| Weaknesses | Can't explain business logic or context | More expensive to maintain under frequent API changes |
| Typical use case | Internal/partner APIs that change quickly | Public APIs where developer onboarding is high-stakes |
