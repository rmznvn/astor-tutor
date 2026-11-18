# Task 23 — Explanation

## Architecture
Create a reusable Astro gallery with optimized thumbnails and a native dialog viewer, keyboard controls and focus restoration. Reuse the existing content model and shared layout. Prefer build-time processing for static data, and browser-side scripts only for genuine interaction.

## Interview questions and strong candidate answers
### Why native dialog?
**Answer:** showModal provides a top-layer modal, Escape handling and focus management with less custom code.

### How to avoid listener duplication?
**Answer:** Initialize once per component instance and abort handlers on navigation.

### How to prevent layout shift?
**Answer:** Reserve aspect ratios and use optimized images with explicit dimensions.

## Common pitfalls
- Creating duplicate state rather than deriving it.
- Inaccessible custom controls or missing focus restoration.
- Unescaped user content in HTML strings.
- Unnecessary hydration or expensive browser bundles.
- Forgetting the zero-results and no-JS cases.

## Alternative architecture
A React island may be appropriate for gestures, zoom and annotations, but native dialog minimizes JavaScript for a simple viewer.

## CSS reasoning
CSS scroll snap, aspect-ratio, object-fit, reduced-motion treatment. Test the design at narrow and wide widths and explain how it degrades when a progressive CSS feature is unavailable.
