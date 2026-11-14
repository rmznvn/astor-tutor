# Task 13 — Astro Actions & Feedback Form

## Scenario
Build a progressively enhanced feedback form using Astro Actions with schema validation and an accessible success/error state.

## Requirements
- Create `src/actions/index.ts` and define a `sendFeedback` action.
- Validate name, email, rating, and message with `astro/zod`.
- Submit from a real HTML form using an Astro Action.
- Render field-level errors and a form-level message.
- Keep a no-JS form submission path.
- Do not expose secrets or send data to a real external API in the reference task.

## Modern CSS practice
Use CSS `:user-invalid`, `:has()`, form field grouping, `accent-color`, and a small success animation.

## Accessibility & performance
- Preserve semantic HTML and keyboard usability.
- Avoid unnecessary hydration and large client payloads.
- Respect `prefers-reduced-motion` where animation is used.
- Test at 320px and desktop widths.

## Acceptance criteria
- [ ] Create `src/actions/index.ts` and define a `sendFeedback` action.
- [ ] Validate name, email, rating, and message with `astro/zod`.
- [ ] Submit from a real HTML form using an Astro Action.
- [ ] Render field-level errors and a form-level message.
- [ ] Keep a no-JS form submission path.
- [ ] Do not expose secrets or send data to a real external API in the reference task.
- [ ] `npm run build` succeeds.
- [ ] Existing Tasks 01–12 still work.

## Hints
1. Start from the Astro/server boundary before styling.
2. Keep the smallest possible browser-side API surface.
3. Prefer platform features and Astro primitives before adding another dependency.

## Stretch goals
- Add automated checks for the feature.
- Measure the network/client-JS impact.
- Document one alternative architecture and when you would choose it.
