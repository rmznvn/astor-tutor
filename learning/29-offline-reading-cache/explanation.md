# Explanation: Offline Reading Pack with Service Worker

## How to reason
Users want to save a small set of already-public article pages for offline reading without converting the Astro site into a full PWA. The central decision is where each responsibility belongs. Use Astro for server/build-time markup, pure TypeScript for validation/transforms, and a minimal browser script only when interaction is necessary.

## Suggested implementation sequence
1. Sketch semantic HTML and expected states.
2. Implement data validation or request logic before styling.
3. Wire the interaction and accessible feedback.
4. Apply the CSS technique: CSS `:has()` for status-aware controls with a class fallback, `color-scheme`, and reduced-motion transitions.
5. Test with keyboard, narrow viewport, reduced motion and error scenarios.

## Interview questions and model answers
### Why is service-worker caching different from localStorage?
**Candidate answer:** Cache API stores Request/Response objects and can serve navigation responses when offline; localStorage stores strings and cannot intercept requests.

### Why limit cache scope and size?
**Candidate answer:** Unbounded storage and caching personalized pages risk privacy and quota problems; a small explicit allowlist is safer.

### How do you test offline support?
**Candidate answer:** Save while online, then simulate offline in browser devtools and navigate to the exact cached route; test missing pages separately.

## Common mistakes
- Assuming Astro components hydrate automatically: browser interactions need explicit client scripts or hydrated islands.
- Styling before confirming semantics and data flow.
- Testing only the success path.
- Treating a reference snippet as production-verified without running it in the actual project.

## Alternative approach
Use a dedicated PWA integration/workbox for larger precache manifests, background updates and complex versioning; the manual worker is educational and intentionally limited.
