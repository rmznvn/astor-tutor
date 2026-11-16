# Task 20 — Concepts and interview preparation

## Rendering boundary
Astro renders the card/tray markup at build time from the content collection. The browser owns the visitor's selection because it is personal, ephemeral UI state. A share link is a portable **snapshot**, not a backend account. The browser script is bundled once by Astro; no hydrated React island is needed.

## State precedence
On page load, an explicitly present `saved` query parameter wins over personal storage, including an empty value. This prevents a recipient's own reading list from silently changing a shared selection. Otherwise use personal storage. Clicking Save edits the **current view** and writes the resulting IDs to storage; merely opening a shared URL does not write storage.

## Parsing and URL safety
Treat query parameters as untrusted input. Keep a catalog-derived allowlist of IDs, deduplicate with `Set`, and limit selections to 20. Use `URL` and `URLSearchParams` to preserve unrelated parameters/hash. Build DOM with `createElement` and `textContent`, not HTML interpolation.

## ClientRouter lifecycle
A normal `DOMContentLoaded` listener is insufficient when Astro replaces page content without a full reload. Initialize on `astro:page-load`. Keep an `AbortController` and abort old listeners before attaching new ones; ensure the module also handles the first visit. The History API does not dispatch `popstate` for calls to `replaceState`, so update UI directly after a click.

## CSS
The tray is a container, so layout responds to its available width rather than the entire viewport. Use logical spacing for writing-mode robustness. `color-mix()` can produce subtle borders while preserving the site's existing color system. A short entry animation is decorative; disable it for reduced motion. Keep a visible focus indicator.

## Interview questions with model answers

**1. Why not store only in localStorage?**  
*“LocalStorage is good for a single browser, but it cannot represent a shareable state. I use a URL snapshot for sharing and localStorage for personal persistence, with explicit precedence so the two don't overwrite each other unexpectedly.”*

**2. Why a Set?**  
*“I need membership checks and uniqueness. A Set prevents duplicate IDs and makes toggle operations straightforward. I validate each ID against the collection-derived catalog before adding it.”*

**3. Why not put all saved cards into a React component?**  
*“The cards already exist as Astro HTML. I only need a small interaction layer, so a bundled script with event delegation avoids shipping a framework runtime for simple selection behavior.”*

**4. What does replaceState do differently from pushState?**  
*“replaceState updates the current history entry without creating a new Back step for every toggle. pushState would be appropriate if each selection change should be independently navigable. Neither automatically fires popstate.”*

**5. How do you handle navigation under ClientRouter?**  
*“I reinitialize after astro:page-load because the DOM can be replaced. I abort listeners from the previous initialization so returning to the page doesn't double-toggle items.”*

**6. What if localStorage or clipboard is unavailable?**  
*“I wrap storage in try/catch and keep the in-memory UI usable. For clipboard failure I show a plain URL the visitor can manually copy, rather than claiming copying succeeded.”*

**7. How do you protect against injection?**  
*“Only known IDs enter the state, and user-visible content is inserted with textContent and DOM nodes. I never inject query-string values as HTML.”*

## Alternative approaches
A React island would be reasonable if the reading list later needed complex search, drag-and-drop reordering, or cross-component state. A server-backed account would be required for multi-device persistence. Neither is justified for this small browser-only feature.

## Common mistakes
- Using `params.get('saved') || stored` (incorrect for `?saved=`).
- Overwriting storage when simply opening a shared link.
- Replacing the entire query string and dropping other filters.
- Rebinding listeners on every ClientRouter visit.
- Putting arbitrary query-string content into `innerHTML`.
