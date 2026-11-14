# Task 12 — Middleware & Request Context

## Scenario
Add request-aware middleware that assigns a request ID, exposes typed locals, and shows a small diagnostic banner only in development.

## Requirements
- Create `src/middleware.ts` with `onRequest()`.
- Generate or reuse an `x-request-id` header and expose it through `locals`.
- Type `App.Locals` in `src/env.d.ts`.
- Render a development-only diagnostic component from the shared layout.
- Do not leak sensitive headers or cookies.
- Preserve all existing routes and ClientRouter behavior.

## Modern CSS practice
Use CSS scope, logical properties, `@supports`, and a non-blocking fixed status badge with reduced-motion support.

## Accessibility & performance
- Preserve semantic HTML and keyboard usability.
- Avoid unnecessary hydration and large client payloads.
- Respect `prefers-reduced-motion` where animation is used.
- Test at 320px and desktop widths.

## Acceptance criteria
- [ ] Create `src/middleware.ts` with `onRequest()`.
- [ ] Generate or reuse an `x-request-id` header and expose it through `locals`.
- [ ] Type `App.Locals` in `src/env.d.ts`.
- [ ] Render a development-only diagnostic component from the shared layout.
- [ ] Do not leak sensitive headers or cookies.
- [ ] Preserve all existing routes and ClientRouter behavior.
- [ ] `npm run build` succeeds.
- [ ] Existing Tasks 01–11 still work.

## Hints
1. Start from the Astro/server boundary before styling.
2. Keep the smallest possible browser-side API surface.
3. Prefer platform features and Astro primitives before adding another dependency.

## Stretch goals
- Add automated checks for the feature.
- Measure the network/client-JS impact.
- Document one alternative architecture and when you would choose it.
