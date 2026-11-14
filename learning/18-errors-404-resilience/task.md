# Task 18 — 404, Error States & Resilient UX

## Scenario
Build a useful custom 404 page plus reusable empty/error components that preserve navigation and help users recover.

## Requirements
- Create `src/pages/404.astro`.
- Include links to Home, Resources, and Search.
- Create a reusable `EmptyState.astro` for no-results/no-content situations.
- Use semantic headings and status messaging.
- Ensure decorative UI does not carry required information.
- Test an invalid resource URL and a zero-result search state.

## Modern CSS / UX practice
Practice CSS `@media (forced-colors: active)`, high-contrast focus states, `min()` typography, and decorative pseudo-elements that disappear in reduced/forced-color modes.

## Accessibility & performance
- Keep semantic structure and keyboard access.
- Avoid shipping JavaScript unless the interaction requires it.
- Test reduced motion and a 320px viewport.
- Verify the feature does not regress previous tasks.

## Acceptance criteria
- [ ] Create `src/pages/404.astro`.
- [ ] Include links to Home, Resources, and Search.
- [ ] Create a reusable `EmptyState.astro` for no-results/no-content situations.
- [ ] Use semantic headings and status messaging.
- [ ] Ensure decorative UI does not carry required information.
- [ ] Test an invalid resource URL and a zero-result search state.
- [ ] `npm run build` succeeds.

## Hints
1. Identify which work belongs at build time, request time, and browser time.
2. Keep user-specific data out of public caches and static serialized props.
3. Prefer a small independently testable unit over a large component with mixed responsibilities.

## Stretch goals
- Add CI for the new checks where appropriate.
- Record one performance measurement before and after.
- Add one failure-path test, not only the happy path.
