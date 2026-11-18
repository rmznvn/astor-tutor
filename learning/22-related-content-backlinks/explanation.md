# Task 22 — Explanation

## Architecture
Derive a build-time relationship graph from the existing content collection and render related articles and incoming references. Reuse the existing content model and shared layout. Prefer build-time processing for static data, and browser-side scripts only for genuine interaction.

## Interview questions and strong candidate answers
### Why build the graph at build time?
**Answer:** Static content is already available; build-time computation gives crawlable links without network requests.

### Why use Sets and Maps?
**Answer:** They deduplicate references and make ID lookup efficient.

### How do you prevent unstable recommendations?
**Answer:** Use explicit priority, tag overlap and alphabetical tie-breaking.

## Common pitfalls
- Creating duplicate state rather than deriving it.
- Inaccessible custom controls or missing focus restoration.
- Unescaped user content in HTML strings.
- Unnecessary hydration or expensive browser bundles.
- Forgetting the zero-results and no-JS cases.

## Alternative architecture
For very large collections, generate a precomputed relationship index once during build and reuse it across pages.

## CSS reasoning
Container queries, logical properties, responsive card grids. Test the design at narrow and wide widths and explain how it degrades when a progressive CSS feature is unavailable.
