# Task 11 — Global Search & Command Palette

**Level:** Intermediate+ · **Time:** 3–5 hours  
**Scenario:** A documentation/knowledge-base site needs fast global navigation without sending all article bodies to the browser.

## Goal
Build a reusable search command palette opened by **⌘K on macOS / Ctrl+K on Windows**, or by a visible Search button. Search resource titles, descriptions, and topics. Support keyboard selection and direct navigation to existing resource pages.

## Astro / React skills
- Query `getCollection('resources')` in an Astro component.
- Map entries to a **small, serializable** search index (id, title, description, topic, url).
- Pass the index as props to one React island using `client:idle`.
- Use React controlled input, derived filtering, `useMemo`, `useEffect`, `useRef`.
- Integrate with the existing `/resources/[slug]` routes.
- Avoid fetching or hydrating the article body.

## Requirements
1. A visible Search button opens the palette; ⌘K / Ctrl+K also opens it.
2. Search is case-insensitive and trims whitespace.
3. Show up to 8 matching resources, plus a result count.
4. Show topic, title and concise description for each result.
5. ArrowDown / ArrowUp move the active option, Enter opens it, Escape closes.
6. Clicking a result navigates to its existing URL.
7. Empty query shows a short set of suggested resources.
8. No matches shows an accessible empty state.
9. Focus moves into search on opening, returns to the trigger on closing, and stays within the modal while open.
10. The feature survives Astro ClientRouter navigation without duplicate keyboard handlers.
11. If React has not hydrated yet, the trigger remains a functional link to `/resources` (progressive enhancement).

## Suggested files
```text
src/
├── components/
│   ├── GlobalSearch.astro
│   └── SearchPalette.tsx
└── styles/
    └── search-palette.css
```

## CSS practice
Practice **CSS cascade layers** and a **fluid modal with dynamic viewport units**:
- `@layer components`, `min()`, `clamp()`, `100dvh`, logical properties.
- `::backdrop` with `backdrop-filter`.
- `:focus-visible` and `[aria-selected="true"]`.
- An entrance animation using opacity and transform; honor `prefers-reduced-motion`.
- Test 320px width and a mobile virtual keyboard.

## Accessibility & performance
- Prefer native `<dialog>` for modal focus containment.
- Use a combobox + listbox relationship or a clearly documented alternative; test screen-reader labels.
- Never trap keyboard focus when closed.
- Use `textContent`-equivalent React rendering; never inject raw HTML for highlighting.
- Hydrate only the palette, not the entire page.
- The keyboard shortcut must not override browser shortcuts while typing into another editable field unless it is the explicit ⌘K/Ctrl+K shortcut.

## Acceptance criteria
- [ ] `npm run build` succeeds.
- [ ] Works with mouse, touch, keyboard and screen reader.
- [ ] Focus management, Escape, Enter, and arrows work.
- [ ] No duplicate shortcut listeners after repeated navigation.
- [ ] Results link to real resource routes.
- [ ] No body/Markdown source sent as search-index props.
- [ ] Responsive at 320px, reduced motion supported.
- [ ] Without JS, the visible trigger still navigates to the resource archive.

## Hints — no full solution
1. Keep `getCollection()` on the Astro side; React only receives plain objects.
2. Consider `<a href="/resources">` as the unhydrated fallback trigger.
3. Use one global keydown listener with cleanup in an effect.
4. Store the selected index, not a duplicate selected resource object.
5. Reset the index when the query changes.
6. The native dialog API provides modal focus behavior; remember to restore focus after close.
7. If ClientRouter replaces the island, React's effect cleanup should remove listeners.

## Stretch goals
- Highlight matched terms without unsafe HTML.
- Group results by topic.
- Add a keyboard-help footer with ⌘K / Ctrl+K hints.
- Add a no-JS dedicated search page.
- Replace simple substring matching with a tested fuzzy-search utility.
- Test 1000+ records and measure search-index payload size.
