# Task 20 — Shareable Reading List

## Product scenario
A documentation visitor bookmarks several resources for later and sends a teammate a link to that selection. The teammate should see the shared selection without signing in. The visitor's own saved items survive a reload.

## Requirements
1. Build a reusable Astro component taking a typed array of resource records: `{ id, title, href }`. Derive records from the **existing** content collection; don't hard-code production content.
2. Render one accessible Save/Remove toggle per resource, a visible saved count, and a tray containing links to selected resources.
3. Store personal selections in `localStorage` under a versioned key. Read/write defensively: blocked storage must not crash the page.
4. Use `?saved=id1,id2` as the **shareable selection**. Parse safely, ignore unknown IDs, remove duplicates, and limit to 20. An explicitly present empty `?saved=` means an empty selection.
5. Share button copies the current selection URL. Show a useful failure message when clipboard access is unavailable. **Do not overwrite the personal localStorage selection merely because a shared link was opened.**
6. Keep `history.replaceState` updates scoped to the `saved` query parameter; preserve other filters and the hash. Listen for `popstate`.
7. Support Astro `ClientRouter` navigations: the UI must initialize on `astro:page-load` and avoid duplicate event listeners.
8. Add a responsive, container-query-based tray. Practice CSS `:focus-visible`, logical properties, `color-mix()`, and a restrained entry animation with `prefers-reduced-motion` fallback.
9. Render meaningful HTML before JavaScript runs; JS enhances the buttons and selection state. Do not add React for this task.

## Acceptance criteria
- [ ] A saved resource appears exactly once in the tray.
- [ ] Saving/removing updates count, button label, `aria-pressed`, and tray.
- [ ] Reload restores the visitor's personal selection when there is no `saved` parameter.
- [ ] A share URL opens the shared selection even when the recipient has different saved items.
- [ ] `?saved=` displays zero selected items; invalid IDs and duplicates are ignored.
- [ ] Other URL parameters and hash survive Save and Share.
- [ ] Back/forward navigation and ClientRouter visits don't duplicate handlers.
- [ ] Storage denial, clipboard denial, keyboard-only use, and 320px viewport are handled.
- [ ] No extra client framework or unnecessary hydration.
- [ ] `npm run build` passes after integration.

## Interview-style hints — no solution
1. Which data is canonical: the resource catalog, the personal saved IDs, or the shared URL? What should win on initial load?
2. How can you turn an untrusted query string into valid IDs without trusting arbitrary HTML?
3. Why is a `Set` useful, and why should its size be capped?
4. Can you attach one click listener to a parent instead of one per card? What happens when ClientRouter replaces the page?
5. Why does `replaceState` not fire `popstate`? When would `pushState` be preferable?
6. How can you update text and links safely without assigning untrusted HTML?

## Edge cases
- URL `?saved=unknown,a,a`, empty URL value, and more than 20 IDs.
- `localStorage` throws `SecurityError`; clipboard API missing; page loaded without JS.
- Re-entering the page through ClientRouter after navigating away.

## Stretch goals
- Write pure parser/serializer unit tests (reuse Vitest from Task 19).
- Add an explicit “Save shared list to my device” action, keeping shared and personal data separate.
- Add a polite empty-state illustration using CSS only, no large image asset.
- Compare URL query state with URL hash state and justify the choice.
