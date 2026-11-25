# Task 32 — Custom Astro Integration & Build Metrics

## Scenario
Create a local Astro integration that reports generated page sizes after a static build, plus a native interactive report viewer using CSS meter styling.

## Requirements
Register a local Astro integration using astro:build:done. Walk the output directory for HTML files, measure bytes, sort largest first and emit a JSON report outside the public output. Create a static report viewer that accepts a local JSON file and validates its shape before rendering. Use native meter elements, safe DOM APIs and CSS grid with minmax. Keep the report out of production assets.

## Acceptance criteria
- [ ] Build emits report at project root
- [ ] paths are relative
- [ ] report is sorted
- [ ] valid file displays sizes
- [ ] malformed file gives a status message
- [ ] HTML-like paths render as text
- [ ] no network request or external dependency.
- [ ] npm run build succeeds and previous routes still work.

## Hints (before coding)
Which hook runs after output is written? How can Node filesystem APIs recurse through directories? Why should uploaded JSON be treated as untrusted data? How can a meter represent a byte budget?

## Stretch goals
Add gzip sizes using node:zlib; fail CI on a configurable budget; compare two reports.
