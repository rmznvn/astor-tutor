# Task 28: Responsive Feature Comparison from Content

## Product scenario
A frontend knowledge hub needs a comparison matrix for Astro, React and Webflow techniques, authored in typed content and usable on narrow screens.

## Requirements
- Create typed product/feature records and normalize missing values predictably.
- Render semantic table caption, row/column headers and no client hydration.
- Support keyboard scrolling at 320px without clipping or inaccessible hidden columns.
- Explore CSS subgrid in an optional card comparison view without breaking native table semantics.
- Use realistic comparison data and test duplicate/unknown IDs.

## CSS / UI practice
CSS subgrid for aligned comparison rows; `overflow-x:auto` fallback, sticky row headers and scroll shadows. Add meaningful visual feedback without relying on color alone.

## Acceptance criteria
- [ ] Create typed product/feature records and normalize missing values predictably.
- [ ] Render semantic table caption, row/column headers and no client hydration.
- [ ] Support keyboard scrolling at 320px without clipping or inaccessible hidden columns.
- [ ] Explore CSS subgrid in an optional card comparison view without breaking native table semantics.
- [ ] Use realistic comparison data and test duplicate/unknown IDs.
- [ ] Keyboard navigation and screen-reader labels work.
- [ ] No unnecessary client-side hydration; verify mobile at 320px.
- [ ] Respect reduced motion, test failure states and confirm `npm run build` succeeds.

## Hints (do not open the solution yet)
1. Identify what Astro can render statically and what must run in the browser.
2. Draw the data flow and decide what belongs in a reusable component versus a pure helper.
3. Test a failure case before polishing the happy path.
4. Check focus, keyboard use and 320px overflow.

## Interview rehearsal
- Why keep a semantic table?
- What does subgrid solve?
- Why normalize data before rendering?

## Stretch goals
- Add a small automated test covering the most important edge case.
- Document an alternative architecture and the trade-offs.
- Measure client JavaScript and Lighthouse impact before and after.
