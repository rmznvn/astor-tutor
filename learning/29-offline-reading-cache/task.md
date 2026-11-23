# Task 29: Offline Reading Pack with Service Worker

## Product scenario
Users want to save a small set of already-public article pages for offline reading without converting the Astro site into a full PWA.

## Requirements
- Register a same-origin service worker on HTTPS/localhost and provide a clear unsupported-browser state.
- Cache only public article navigation HTML, never authenticated responses or arbitrary URLs.
- Allow a user-triggered Save action and announce success/error without blocking reading.
- Cap cached pages and version the cache; remove obsolete caches on activation.
- Explain that images/CSS/JS may require separate caching and that first offline load is not guaranteed.
- Test online save, offline navigation, cache eviction and a bad URL.

## CSS / UI practice
CSS `:has()` for status-aware controls with a class fallback, `color-scheme`, and reduced-motion transitions. Add meaningful visual feedback without relying on color alone.

## Acceptance criteria
- [ ] Register a same-origin service worker on HTTPS/localhost and provide a clear unsupported-browser state.
- [ ] Cache only public article navigation HTML, never authenticated responses or arbitrary URLs.
- [ ] Allow a user-triggered Save action and announce success/error without blocking reading.
- [ ] Cap cached pages and version the cache; remove obsolete caches on activation.
- [ ] Explain that images/CSS/JS may require separate caching and that first offline load is not guaranteed.
- [ ] Test online save, offline navigation, cache eviction and a bad URL.
- [ ] Keyboard navigation and screen-reader labels work.
- [ ] No unnecessary client-side hydration; verify mobile at 320px.
- [ ] Respect reduced motion, test failure states and confirm `npm run build` succeeds.

## Hints (do not open the solution yet)
1. Identify what Astro can render statically and what must run in the browser.
2. Draw the data flow and decide what belongs in a reusable component versus a pure helper.
3. Test a failure case before polishing the happy path.
4. Check focus, keyboard use and 320px overflow.

## Interview rehearsal
- Why is service-worker caching different from localStorage?
- Why limit cache scope and size?
- How do you test offline support?

## Stretch goals
- Add a small automated test covering the most important edge case.
- Document an alternative architecture and the trade-offs.
- Measure client JavaScript and Lighthouse impact before and after.
