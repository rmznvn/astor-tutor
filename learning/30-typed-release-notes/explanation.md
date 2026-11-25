# Task 30 — Typed Release Notes & Native Disclosure

## Concepts and reasoning
The glob loader reads local Markdown at build time; Zod validates frontmatter before rendering. getCollection returns entries, while render produces the Markdown component. Native details keeps state in the browser without hydration. Logical border properties work in both writing directions.

## Interview questions and sample answers
**Why use a collection rather than importing Markdown manually?** I want validated, queryable entries and consistent authoring.

**Why not hydrate a React accordion?** Native details already supports keyboard interaction and open state; I would hydrate only for additional requirements.

**Why sort explicitly?** Loader order is not a publication-order contract; a tie-breaker makes builds reproducible.

## Alternative approach
Use a JSON collection for short plain-text updates; Markdown is preferable when editors need formatted release notes.
