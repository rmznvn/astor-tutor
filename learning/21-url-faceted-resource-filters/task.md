# Task 21 — URL-first Faceted Resource Filters

## Scenario
Build shareable topic and difficulty filters on the resource library using GET form controls and URLSearchParams.

## Requirements
1. Use a semantic GET form with repeatable topic checkboxes and a difficulty select.
2. Initialize controls from URLSearchParams and update URL on submit.
3. Filter collection summaries with an accessible live result count and empty state.
4. Handle popstate and Astro navigation without duplicate listeners.
5. Preserve keyboard access, responsive layouts and reduced motion.

## Acceptance criteria
- [ ] Use a semantic GET form with repeatable topic checkboxes and a difficulty select.
- [ ] Initialize controls from URLSearchParams and update URL on submit.
- [ ] Filter collection summaries with an accessible live result count and empty state.
- [ ] Handle popstate and Astro navigation without duplicate listeners.
- [ ] Preserve keyboard access, responsive layouts and reduced motion.
- [ ] Existing pages still work; `npm run build` passes.
- [ ] Keyboard, reduced motion, 320px mobile and desktop are tested.

## Interview hints — without the solution
1. Identify the source of truth and the minimum required state.
2. Separate build-time Astro data processing from browser interaction.
3. Sketch semantic HTML and edge cases before coding.
4. Explain trade-offs before adding a framework or dependency.

## Edge cases
- Empty collection and missing optional fields.
- Repeated entries, invalid query/reference data, and keyboard-only use.
- Navigation between pages with Astro ClientRouter.

## Modern CSS
CSS subgrid, :has(), auto-fit grid

## Stretch goals
- Add removable selected-filter chips.
- Add URL refresh and browser-back Playwright tests.
