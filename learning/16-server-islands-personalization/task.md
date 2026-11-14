# Task 16 — Server Islands & Personalized UI

## Scenario
Keep the resource page static while rendering a small personalized server-side panel independently with `server:defer` and a fallback.

## Requirements
- Install/configure an adapter that supports on-demand rendering.
- Create a server-rendered Astro component used with `server:defer`.
- Pass only small serializable props to the island.
- Render a meaningful `slot="fallback"` placeholder.
- Simulate personalized content from a request cookie or safe request data.
- Do not make the whole page dynamic just for one personalized block.

## Modern CSS / UX practice
Practice skeleton UI, `content-visibility`, `contain-intrinsic-size`, and a shimmer-free reduced-motion loading state.

## Accessibility & performance
- Keep semantic structure and keyboard access.
- Avoid shipping JavaScript unless the interaction requires it.
- Test reduced motion and a 320px viewport.
- Verify the feature does not regress previous tasks.

## Acceptance criteria
- [ ] Install/configure an adapter that supports on-demand rendering.
- [ ] Create a server-rendered Astro component used with `server:defer`.
- [ ] Pass only small serializable props to the island.
- [ ] Render a meaningful `slot="fallback"` placeholder.
- [ ] Simulate personalized content from a request cookie or safe request data.
- [ ] Do not make the whole page dynamic just for one personalized block.
- [ ] `npm run build` succeeds.

## Hints
1. Identify which work belongs at build time, request time, and browser time.
2. Keep user-specific data out of public caches and static serialized props.
3. Prefer a small independently testable unit over a large component with mixed responsibilities.

## Stretch goals
- Add CI for the new checks where appropriate.
- Record one performance measurement before and after.
- Add one failure-path test, not only the happy path.
