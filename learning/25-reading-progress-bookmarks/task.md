# Task 25 — Accessible Reading Progress & Section Bookmarks

## Scenario
Readers need a reliable progress indicator and shareable section links on long articles, including after ClientRouter navigation.

## Requirements
1. Build a reusable `ReadingProgress.astro` component with native `<progress>`, label and output.
2. Measure progress against article height, clamp 0–100, and handle short articles.
3. Batch scroll/resize updates with `requestAnimationFrame`.
4. Remove listeners on `astro:before-swap`; reinitialize on `astro:page-load`.
5. Add keyboard-accessible heading permalinks with unique IDs.
6. Practice `scroll-margin-block-start`, `:focus-visible`, `accent-color` and progressive CSS scroll-driven animation (optional, support-detected).

## Acceptance criteria
- [ ] Screen reader announces progress element label/value.
- [ ] No duplicate event listeners after navigation.
- [ ] Heading permalink opens the intended section.
- [ ] 320px and reduced-motion behavior remain usable.
- [ ] `npm run build` passes.

## Interview hints
What is the source of truth for progress? Why not store scroll state in React? What cleanup is necessary during client navigation? How would you test the short-article case?

## Stretch goals
Add Playwright navigation tests, support a sticky header of variable height, and compare IntersectionObserver-based progress.
