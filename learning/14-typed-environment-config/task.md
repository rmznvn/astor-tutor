# Task 14 — Typed Environment Configuration

## Scenario
Introduce type-safe public/server environment variables and build a reusable environment/status component without leaking server secrets.

## Requirements
- Define an `env.schema` in `astro.config.mjs`.
- Create at least one server secret and one public variable.
- Import server values only in server/Astro code.
- Use a public variable in browser-visible UI.
- Add `.env.example` without real secrets.
- Explain why `PUBLIC_` variables are not secret.

## Modern CSS practice
Practice `@property`, custom properties, `color-mix()`, and environment-specific visual tokens.

## Accessibility & performance
- Preserve semantic HTML and keyboard usability.
- Avoid unnecessary hydration and large client payloads.
- Respect `prefers-reduced-motion` where animation is used.
- Test at 320px and desktop widths.

## Acceptance criteria
- [ ] Define an `env.schema` in `astro.config.mjs`.
- [ ] Create at least one server secret and one public variable.
- [ ] Import server values only in server/Astro code.
- [ ] Use a public variable in browser-visible UI.
- [ ] Add `.env.example` without real secrets.
- [ ] Explain why `PUBLIC_` variables are not secret.
- [ ] `npm run build` succeeds.
- [ ] Existing Tasks 01–13 still work.

## Hints
1. Start from the Astro/server boundary before styling.
2. Keep the smallest possible browser-side API surface.
3. Prefer platform features and Astro primitives before adding another dependency.

## Stretch goals
- Add automated checks for the feature.
- Measure the network/client-JS impact.
- Document one alternative architecture and when you would choose it.
