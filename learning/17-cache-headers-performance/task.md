# Task 17 — Caching, Headers & Performance Budget

## Scenario
Create a cache strategy for static assets, JSON endpoints, and dynamic responses, then document and verify a simple performance budget.

## Requirements
- Add intentional `Cache-Control` headers to the JSON endpoint from Task 08.
- Use middleware or endpoint response headers without caching personalized content publicly.
- Add a small performance-budget document/checklist.
- Measure JS, image, and HTML payloads in build/preview.
- Apply `content-visibility` only where it does not harm important above-the-fold content.
- Document stale-content trade-offs.

## Modern CSS / UX practice
Use `content-visibility: auto`, `contain-intrinsic-size`, and responsive intrinsic layouts to reduce rendering work below the fold.

## Accessibility & performance
- Keep semantic structure and keyboard access.
- Avoid shipping JavaScript unless the interaction requires it.
- Test reduced motion and a 320px viewport.
- Verify the feature does not regress previous tasks.

## Acceptance criteria
- [ ] Add intentional `Cache-Control` headers to the JSON endpoint from Task 08.
- [ ] Use middleware or endpoint response headers without caching personalized content publicly.
- [ ] Add a small performance-budget document/checklist.
- [ ] Measure JS, image, and HTML payloads in build/preview.
- [ ] Apply `content-visibility` only where it does not harm important above-the-fold content.
- [ ] Document stale-content trade-offs.
- [ ] `npm run build` succeeds.

## Hints
1. Identify which work belongs at build time, request time, and browser time.
2. Keep user-specific data out of public caches and static serialized props.
3. Prefer a small independently testable unit over a large component with mixed responsibilities.

## Stretch goals
- Add CI for the new checks where appropriate.
- Record one performance measurement before and after.
- Add one failure-path test, not only the happy path.
