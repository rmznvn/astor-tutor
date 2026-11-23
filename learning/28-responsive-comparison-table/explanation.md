# Explanation: Responsive Feature Comparison from Content

## How to reason
A frontend knowledge hub needs a comparison matrix for Astro, React and Webflow techniques, authored in typed content and usable on narrow screens. The central decision is where each responsibility belongs. Use Astro for server/build-time markup, pure TypeScript for validation/transforms, and a minimal browser script only when interaction is necessary.

## Suggested implementation sequence
1. Sketch semantic HTML and expected states.
2. Implement data validation or request logic before styling.
3. Wire the interaction and accessible feedback.
4. Apply the CSS technique: CSS subgrid for aligned comparison rows; `overflow-x:auto` fallback, sticky row headers and scroll shadows.
5. Test with keyboard, narrow viewport, reduced motion and error scenarios.

## Interview questions and model answers
### Why keep a semantic table?
**Candidate answer:** The row/column relationship is essential; native table headers convey associations to assistive technology.

### What does subgrid solve?
**Candidate answer:** It lets nested grid items align to ancestor tracks; it is useful for card comparisons but not a reason to replace a semantic table.

### Why normalize data before rendering?
**Candidate answer:** Central validation catches invalid IDs and ensures consistent missing-cell output across all views.

## Common mistakes
- Assuming Astro components hydrate automatically: browser interactions need explicit client scripts or hydrated islands.
- Styling before confirming semantics and data flow.
- Testing only the success path.
- Treating a reference snippet as production-verified without running it in the actual project.

## Alternative approach
Use CSS-only stacked cards instead of a scrollable table for small comparisons; preserve explicit labels for every value, and prefer a table for many columns.
