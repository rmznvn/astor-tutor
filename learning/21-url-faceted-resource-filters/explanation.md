# Task 21 — Explanation

## Architecture
Build shareable topic and difficulty filters on the resource library using GET form controls and URLSearchParams. Reuse the existing content model and shared layout. Prefer build-time processing for static data, and browser-side scripts only for genuine interaction.

## Interview questions and strong candidate answers
### Why store filters in the URL?
**Answer:** The URL is shareable, survives refresh, and avoids duplicated state.

### Why a GET form?
**Answer:** It provides semantic, keyboard-friendly controls and a progressive enhancement path.

### How to avoid injection?
**Answer:** Create DOM nodes and assign textContent instead of injecting untrusted HTML.

## Common pitfalls
- Creating duplicate state rather than deriving it.
- Inaccessible custom controls or missing focus restoration.
- Unescaped user content in HTML strings.
- Unnecessary hydration or expensive browser bundles.
- Forgetting the zero-results and no-JS cases.

## Alternative architecture
For a very large dataset, use an on-demand server-rendered GET page rather than serializing every resource to the client.

## CSS reasoning
CSS subgrid, :has(), auto-fit grid. Test the design at narrow and wide widths and explain how it degrades when a progressive CSS feature is unavailable.
