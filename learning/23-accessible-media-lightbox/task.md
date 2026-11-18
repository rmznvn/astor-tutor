# Task 23 — Accessible Screenshot Gallery

## Scenario
Create a reusable Astro gallery with optimized thumbnails and a native dialog viewer, keyboard controls and focus restoration.

## Requirements
1. Accept typed image metadata and descriptive alt text as props.
2. Render responsive lazy thumbnails with astro:assets Image.
3. Open a native dialog with previous/next/close controls.
4. Support arrow keys and Escape and restore focus on close.
5. Handle ClientRouter page changes without duplicate handlers.
6. Use CSS scroll snap, accessible focus indicators and reduced-motion styling.

## Acceptance criteria
- [ ] Accept typed image metadata and descriptive alt text as props.
- [ ] Render responsive lazy thumbnails with astro:assets Image.
- [ ] Open a native dialog with previous/next/close controls.
- [ ] Support arrow keys and Escape and restore focus on close.
- [ ] Handle ClientRouter page changes without duplicate handlers.
- [ ] Use CSS scroll snap, accessible focus indicators and reduced-motion styling.
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
CSS scroll snap, aspect-ratio, object-fit, reduced-motion treatment

## Stretch goals
- Show a visible image counter.
- Add responsive full-size srcset.
- Test keyboard and focus restoration with Playwright.
