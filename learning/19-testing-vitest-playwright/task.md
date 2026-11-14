# Task 19 — Testing with Vitest & Playwright

## Scenario
Add a small automated test suite for pure search logic and critical end-to-end flows from the existing Astro project.

## Requirements
- Configure Vitest with Astro's Vite config helper.
- Extract search matching from Task 11 into a pure function and unit test it.
- Add Playwright and test a critical route.
- Test opening the command palette and keyboard navigation.
- Test a 404 route.
- Add at least one mobile viewport assertion for horizontal overflow.
- Keep tests deterministic and independent of network APIs.

## Modern CSS / UX practice
Create no new visual feature; instead test CSS/UX behavior such as visible focus and responsive overflow at a mobile viewport.

## Accessibility & performance
- Keep semantic structure and keyboard access.
- Avoid shipping JavaScript unless the interaction requires it.
- Test reduced motion and a 320px viewport.
- Verify the feature does not regress previous tasks.

## Acceptance criteria
- [ ] Configure Vitest with Astro's Vite config helper.
- [ ] Extract search matching from Task 11 into a pure function and unit test it.
- [ ] Add Playwright and test a critical route.
- [ ] Test opening the command palette and keyboard navigation.
- [ ] Test a 404 route.
- [ ] Add at least one mobile viewport assertion for horizontal overflow.
- [ ] Keep tests deterministic and independent of network APIs.
- [ ] `npm run build` succeeds.

## Hints
1. Identify which work belongs at build time, request time, and browser time.
2. Keep user-specific data out of public caches and static serialized props.
3. Prefer a small independently testable unit over a large component with mixed responsibilities.

## Stretch goals
- Add CI for the new checks where appropriate.
- Record one performance measurement before and after.
- Add one failure-path test, not only the happy path.
