# Explanation: Accessible Code Snippet Toolbar

## How to reason
Documentation readers need copyable code examples with visible language labels, a confirmation message and robust navigation when Astro ClientRouter swaps pages. The central decision is where each responsibility belongs. Use Astro for server/build-time markup, pure TypeScript for validation/transforms, and a minimal browser script only when interaction is necessary.

## Suggested implementation sequence
1. Sketch semantic HTML and expected states.
2. Implement data validation or request logic before styling.
3. Wire the interaction and accessible feedback.
4. Apply the CSS technique: CSS anchor positioning with a flexbox fallback and `@supports`; focus-visible and logical properties.
5. Test with keyboard, narrow viewport, reduced motion and error scenarios.

## Interview questions and model answers
### Why use textContent instead of innerHTML?
**Candidate answer:** textContent preserves literal code and avoids treating markup as executable HTML; copying innerHTML would include tags.

### What happens to event listeners with ClientRouter?
**Candidate answer:** DOM nodes may be replaced; delegation on document remains valid, whereas handlers attached to replaced elements are lost.

### How do you handle clipboard permission errors?
**Candidate answer:** Catch rejected promises, announce failure via role=status and leave the code selectable as a fallback.

## Common mistakes
- Assuming Astro components hydrate automatically: browser interactions need explicit client scripts or hydrated islands.
- Styling before confirming semantics and data flow.
- Testing only the success path.
- Treating a reference snippet as production-verified without running it in the actual project.

## Alternative approach
Use a hydrated React code viewer when complex live editing is required; otherwise Astro plus a tiny delegated listener ships less JavaScript.
