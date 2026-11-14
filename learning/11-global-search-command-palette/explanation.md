# Task 11 — Explanation and interview preparation

## 1. Astro/React boundary
The resource collection is available at build time. React needs only a small index, not Markdown bodies or server-side collection APIs.

```text
getCollection('resources') (Astro/build)
  → project public metadata
  → serialize props
  → SearchPalette (React island)
  → filtered results + keyboard state
```

This keeps the site static-first. No API request is required for a small index.

## 2. Why `client:idle`?
The overlay is not essential to the first paint. Hydrating it when the browser is idle can reduce initial main-thread competition. A real `/resources` link remains available before hydration. If instant keyboard response is a stronger requirement, `client:load` is a valid trade-off.

## 3. Why derived results?
Results depend entirely on the index and query. Store the query, then compute results. Storing a second results array in state risks synchronization bugs.

## 4. Why a native dialog?
`showModal()` handles modal semantics, background inertness and focus containment. It does not automatically implement listbox arrow navigation or a correct active-option announcement. Those remain your responsibility.

## 5. Keyboard model
Use a combobox input with `aria-controls`, `aria-expanded`, and `aria-activedescendant`. Each result has `role="option"` and a stable ID. Keep actual DOM focus in the input while arrow keys change the active descendant. Enter activates the selected result. Escape closes the dialog.

## 6. Astro ClientRouter lifecycle
React effects attach and clean up listeners when the island mounts/unmounts. Avoid adding a second unmanaged global listener from an Astro script. Test back/forward and repeated navigations.

## 7. Why a small index?
Passing complete article bodies as props inflates HTML and hydration data. A few metadata fields make the UI responsive and reduce network and parse cost. A very large site might instead use a generated JSON index, server endpoint, or dedicated search service.

## 8. Common pitfalls
- A `button` that does nothing until hydration.
- Using array index as the only identity for dynamic result nodes.
- Adding keydown listeners on every render.
- Losing focus after the dialog closes.
- Using `role="listbox"` without a matching active-descendant model.
- Using `window.location.href` without considering ClientRouter (normal navigation is still a valid fallback).
- Animating layout properties instead of opacity/transform.

## Interview questions with strong answers

**Q: Why not query Content Collections directly inside React?**  
**A:** Content Collection APIs are designed for Astro's build/server context. I project a minimal serializable index and pass it into an island, keeping the content source and browser UI separate.

**Q: Why use `useMemo`?**  
**A:** It documents that results are derived from props and query, and avoids repeated filtering on unrelated renders. For a tiny index, a direct computation would also be fine.

**Q: Why use a native dialog instead of a custom overlay?**  
**A:** Native dialog provides modal focus handling, backdrop, Escape behavior and background inertness, reducing the accessibility work and custom JavaScript needed.

**Q: What is the difference between `aria-selected` and focus?**  
**A:** Keyboard focus stays in the combobox input. `aria-activedescendant` points to the active option, and `aria-selected` communicates that option's state; they do not move DOM focus to the result.

**Q: How would you scale search to 10,000 resources?**  
**A:** Measure payload and interaction latency first. I might generate a separate searchable index, lazy-load it on demand, use a search library, or move queries to an endpoint. I would not ship all content bodies to the browser.

**Q: How do you ensure it works with Astro page transitions?**  
**A:** React registers global listeners inside effects with cleanup, so navigation does not accumulate handlers. I test multiple route changes and browser history.

## Alternative architecture
For a larger library, generate `/search-index.json` at build time and fetch it on the first search open, caching the result. This reduces initial serialized props at the cost of one request and a loading state. Another option is a server-side search endpoint for frequently updated content.

## References
- https://docs.astro.build/en/guides/framework-components/
- https://docs.astro.build/en/guides/content-collections/
- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog
- https://www.w3.org/WAI/ARIA/apg/patterns/combobox/
