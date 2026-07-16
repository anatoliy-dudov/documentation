# TaskFlow API — auto-generated reference

Below is an interactive reference rendered directly from the
[`openapi.yaml`](openapi.yaml) file using Swagger UI. This isn't rewritten
by hand — if the spec changes, the documentation updates automatically on
the next site build.

## What I actually wrote by hand in this example

A generator builds the structure and parameter tables, but it doesn't write
meaning on the author's behalf. My work as a technical writer here focused
on the specification itself:

- **`summary` and `description` text.** For `DELETE /projects/{projectId}`,
  the description explicitly states this archives the project rather than
  deleting it irreversibly — without that clarification, a developer might
  assume data is permanently lost and avoid calling the method where it's
  actually the right tool.
- **Example values (`example`).** Path parameters and request body fields
  use realistic examples (`proj_8f3ac1`, `task_1029ab`) instead of
  generic `string`/`123` placeholders — this speeds up ad-hoc request
  testing.
- **Error codes and shared responses.** Extracted into
  `components/responses` (`BadRequest`, `Unauthorized`, `NotFound`) so
  error documentation isn't duplicated on every endpoint and stays
  consistent across the API.
- **Enums instead of free-text strings.** The `status` field is defined as
  an `enum` (`todo`, `in_progress`, `blocked`, `done`) rather than an
  arbitrary string — Swagger UI immediately shows developers the valid
  values, and the same constraint doubles as validation.

## Tools and process

- **Spec linting** — e.g. [Spectral](https://github.com/stoplightio/spectral),
  to catch naming-convention violations and missing descriptions before
  review.
- **Spec review as code** — changes to `openapi.yaml` go through a pull
  request like regular code, reviewed by both the technical writer and the
  engineer who owns the endpoint.
- **Rendering** — this site uses the `mkdocs-render-swagger-plugin` MkDocs
  plugin, which embeds Swagger UI directly on the page.

## Interactive reference

!!swagger openapi.yaml!!
