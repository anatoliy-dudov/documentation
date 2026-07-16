# Further reading

Sources and tools I rely on in my work. The list isn't meant to be
exhaustive — it's what was actually used while putting this portfolio
together and in the projects it represents.

## Documentation standards (Russia/CIS)

- **GOST 19 (ESPD)** — the Unified System for Program Documentation.
  Useful as a common language for defining a document set (technical
  specification, user guide, program description), even where it isn't
  formally mandatory.
- **GOST 34** — standards for automated systems. The versions updated in
  2020–2021 should be preferred over older editions when working on
  modern IT projects.
- **ESKD** — the Unified System for Design Documentation. Relevant when
  documentation concerns hardware or physical products rather than
  software alone.

## International approaches and style

- **Google Developer Documentation Style Guide** — a practical style
  guide for technical documentation, especially useful for API
  references.
- **Microsoft Writing Style Guide** — an alternative source of style
  guidance with a strong section on inclusive language and UX writing.
- **Write the Docs** — a technical writers' community; its guides and
  conference talks are a good source of practices proven on real
  products.
- **The Good Docs Project** — open documentation templates (READMEs,
  guides, API reference templates) that are handy as a starting point.

## OpenAPI and API documentation

- **The OpenAPI 3.x specification** — the primary reference for spec
  syntax; useful to keep on hand when reviewing someone else's
  `openapi.yaml`.
- **Spectral** — a linter for OpenAPI/AsyncAPI specs; lets you formalize
  naming conventions and required fields as rules instead of verbal
  agreements.
- **Redoc / Swagger UI** — tools for rendering interactive documentation
  from an OpenAPI spec (this portfolio uses an approach similar to
  `mkdocs-render-swagger-plugin`; see
  [TaskFlow API — generated reference](../api-docs/generated-reference/index.en.md)).

## ITIL and problem management

- **ITIL 4** — a source of terminology and process models for incident
  and problem management (roles, the problem lifecycle, KEDB).
- **The Google SRE Book ("Site Reliability Engineering")** — its chapter
  on postmortem culture is one of the most widely cited sources on
  blameless incident review practice.

## Tools used to build this portfolio

- **MkDocs + MkDocs Material** — the static documentation site generator
  and theme.
- **mkdocs-static-i18n** — the bilingual (RU/EN) navigation plugin for
  MkDocs.
- **Pandoc + XeLaTeX (DejaVu fonts)** — for generating PDF versions of
  materials with correct Cyrillic rendering.
- **Stepik** — the platform used to publish technical-writing courses in
  a lesson/step format.
