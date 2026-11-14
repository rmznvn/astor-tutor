# Task 15 — MDX Component Library

## Scenario
Add MDX support and create reusable documentation components for notes, warnings, code callouts, and embedded interactive examples.

## Requirements
- Install/configure the Astro MDX integration.
- Create reusable `Callout.astro` and `DemoFrame.astro` components.
- Render at least one `.mdx` resource using those components.
- Keep semantic heading order.
- Make code and long URLs wrap safely on mobile.
- Avoid hydrating components that are only presentational.

## Modern CSS practice
Use `@layer`, `:where()`, CSS counters, `code` wrapping strategies, and container-query styling for callouts.

## Accessibility & performance
- Preserve semantic HTML and keyboard usability.
- Avoid unnecessary hydration and large client payloads.
- Respect `prefers-reduced-motion` where animation is used.
- Test at 320px and desktop widths.

## Acceptance criteria
- [ ] Install/configure the Astro MDX integration.
- [ ] Create reusable `Callout.astro` and `DemoFrame.astro` components.
- [ ] Render at least one `.mdx` resource using those components.
- [ ] Keep semantic heading order.
- [ ] Make code and long URLs wrap safely on mobile.
- [ ] Avoid hydrating components that are only presentational.
- [ ] `npm run build` succeeds.
- [ ] Existing Tasks 01–14 still work.

## Hints
1. Start from the Astro/server boundary before styling.
2. Keep the smallest possible browser-side API surface.
3. Prefer platform features and Astro primitives before adding another dependency.

## Stretch goals
- Add automated checks for the feature.
- Measure the network/client-JS impact.
- Document one alternative architecture and when you would choose it.
