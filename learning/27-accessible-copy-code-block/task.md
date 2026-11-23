# Task 27: Accessible Code Snippet Toolbar

## Product scenario
Documentation readers need copyable code examples with visible language labels, a confirmation message and robust navigation when Astro ClientRouter swaps pages.

## Requirements
- Create a reusable component with a language label and named accessible code region.
- Copy exactly the visible code text via Clipboard API, not innerHTML.
- Provide screen-reader status and a useful failure path (HTTPS permissions).
- Keep the listener stable under ClientRouter navigation; avoid attaching one listener per rerender.
- Use CSS anchor positioning as an optional enhancement only, with a working flex fallback.

## CSS / UI practice
CSS anchor positioning with a flexbox fallback and `@supports`; focus-visible and logical properties. Add meaningful visual feedback without relying on color alone.

## Acceptance criteria
- [ ] Create a reusable component with a language label and named accessible code region.
- [ ] Copy exactly the visible code text via Clipboard API, not innerHTML.
- [ ] Provide screen-reader status and a useful failure path (HTTPS permissions).
- [ ] Keep the listener stable under ClientRouter navigation; avoid attaching one listener per rerender.
- [ ] Use CSS anchor positioning as an optional enhancement only, with a working flex fallback.
- [ ] Keyboard navigation and screen-reader labels work.
- [ ] No unnecessary client-side hydration; verify mobile at 320px.
- [ ] Respect reduced motion, test failure states and confirm `npm run build` succeeds.

## Hints (do not open the solution yet)
1. Identify what Astro can render statically and what must run in the browser.
2. Draw the data flow and decide what belongs in a reusable component versus a pure helper.
3. Test a failure case before polishing the happy path.
4. Check focus, keyboard use and 320px overflow.

## Interview rehearsal
- Why use textContent instead of innerHTML?
- What happens to event listeners with ClientRouter?
- How do you handle clipboard permission errors?

## Stretch goals
- Add a small automated test covering the most important edge case.
- Document an alternative architecture and the trade-offs.
- Measure client JavaScript and Lighthouse impact before and after.
