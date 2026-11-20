# Task 26 — Build-time Content Quality Audit

## Scenario
The knowledge hub has many articles. Broken related links, duplicate slugs and missing image alt text should be caught before deployment.

## Requirements
1. Write a pure `auditContent(entries)` function returning typed error/warning findings.
2. Detect duplicate/empty slugs, broken related slugs, missing descriptions and image alt text.
3. Add a static `/audit.json` endpoint using the existing Astro content collection.
4. Add a Node script to read the built audit file and fail on errors (not warnings).
5. Add an `audit:content` package script running the build and checker.
6. Add Vitest tests for duplicate slugs, broken links, and valid content.
7. Build a semantic editorial checklist preview with CSS `:has()` and container queries.

## Acceptance criteria
- [ ] Errors fail the command with a nonzero exit code.
- [ ] Warnings are visible but do not fail the command.
- [ ] No `astro:content` import in plain Node.
- [ ] The output identifies the offending source.
- [ ] Empty collection is handled.
- [ ] Existing project build succeeds.

## Interview hints
How is schema validation different from cross-document auditing? Where can Astro virtual modules be imported? How would you handle false positives for intentionally decorative images?

## Stretch goals
Add a CI workflow, check external URLs with caching and rate limits, and produce a human-readable HTML audit report.
