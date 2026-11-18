# Task 22 — Related Content & Backlinks

## Scenario
Derive a build-time relationship graph from the existing content collection and render related articles and incoming references.

## Requirements
1. Add an optional relatedIds field to the existing resource schema.
2. Compute outgoing references and backlinks while ignoring missing/self IDs.
3. Rank explicit relationships first, shared tags second, and break ties deterministically.
4. Render three related resources plus all backlinks on detail pages.
5. Render useful empty states and accessible links without client JS.

## Acceptance criteria
- [ ] Add an optional relatedIds field to the existing resource schema.
- [ ] Compute outgoing references and backlinks while ignoring missing/self IDs.
- [ ] Rank explicit relationships first, shared tags second, and break ties deterministically.
- [ ] Render three related resources plus all backlinks on detail pages.
- [ ] Render useful empty states and accessible links without client JS.
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
Container queries, logical properties, responsive card grids

## Stretch goals
- Add cycle detection and broken-reference diagnostics.
- Unit-test duplicate references and empty collections.
