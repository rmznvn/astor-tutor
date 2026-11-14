# Task 11 — Complete reference solution

This is a **reference implementation** to compare with your own attempt. It does not modify `src/` automatically.

**Assumptions:** The `resources` collection exists (Task 01), the React integration is installed (Task 03), and detail routes use `/resources/<id>`. If your IDs or routes differ, adapt the URL projection.

## 1. `src/components/GlobalSearch.astro`

```astro
---
import { getCollection } from 'astro:content';
import SearchPalette from './SearchPalette';
import '../styles/search-palette.css';

const resources = await getCollection('resources');

const index = resources.map((entry) => ({
  id: entry.id,
  title: entry.data.title,
  description: entry.data.description,
  topic: entry.data.topic,
  url: `/resources/${entry.id}`,
}));
---

<SearchPalette items={index} client:idle />
```

Place `<GlobalSearch />` in your existing shared layout or header. Do not replace the app layout.

## 2. `src/components/SearchPalette.tsx`

```tsx
import { useEffect, useId, useMemo, useRef, useState } from 'react';

type SearchItem = {
  id: string;
  title: string;
  description: string;
  topic: string;
  url: string;
};

export default function SearchPalette({ items }: { items: SearchItem[] }) {
  const [open, setOpen] = useState(false);
  const [query, setQuery] = useState('');
  const [activeIndex, setActiveIndex] = useState(0);
  const dialogRef = useRef<HTMLDialogElement>(null);
  const inputRef = useRef<HTMLInputElement>(null);
  const triggerRef = useRef<HTMLAnchorElement>(null);
  const listId = useId();
  const statusId = useId();

  const matches = useMemo(() => {
    const normalized = query.trim().toLocaleLowerCase();
    const ranked = [...items].sort((a, b) => a.title.localeCompare(b.title));

    if (!normalized) return ranked.slice(0, 8);

    return ranked.filter((item) =>
      [item.title, item.description, item.topic]
        .some((value) => value.toLocaleLowerCase().includes(normalized))
    ).slice(0, 8);
  }, [items, query]);

  const selected = matches[activeIndex];

  function closePalette() {
    dialogRef.current?.close();
  }

  function openPalette() {
    if (!dialogRef.current || dialogRef.current.open) return;
    setQuery('');
    setActiveIndex(0);
    dialogRef.current.showModal();
    setOpen(true);
    requestAnimationFrame(() => inputRef.current?.focus());
  }

  useEffect(() => {
    const handleShortcut = (event: KeyboardEvent) => {
      if ((event.metaKey || event.ctrlKey) &&
          !event.altKey && event.key.toLowerCase() === 'k') {
        event.preventDefault();
        if (dialogRef.current?.open) closePalette();
        else openPalette();
      }
    };

    window.addEventListener('keydown', handleShortcut);
    return () => window.removeEventListener('keydown', handleShortcut);
  }, []);

  function navigate(url: string) {
    closePalette();
    window.location.assign(url);
  }

  function handleInputKeyDown(event: React.KeyboardEvent<HTMLInputElement>) {
    if (event.key === 'ArrowDown') {
      event.preventDefault();
      setActiveIndex((index) => matches.length ? (index + 1) % matches.length : 0);
    } else if (event.key === 'ArrowUp') {
      event.preventDefault();
      setActiveIndex((index) =>
        matches.length ? (index - 1 + matches.length) % matches.length : 0
      );
    } else if (event.key === 'Enter' && selected) {
      event.preventDefault();
      navigate(selected.url);
    }
  }

  return (
    <>
      <a
        ref={triggerRef}
        href="/resources"
        className="search-trigger"
        onClick={(event) => {
          event.preventDefault();
          openPalette();
        }}
        aria-haspopup="dialog"
      >
        <span>Search resources</span>
        <kbd>⌘K / Ctrl+K</kbd>
      </a>

      <dialog
        ref={dialogRef}
        className="search-dialog"
        aria-label="Search resources"
        onClose={() => {
          setOpen(false);
          triggerRef.current?.focus();
        }}
        onClick={(event) => {
          if (event.target === dialogRef.current) closePalette();
        }}
      >
        <div className="search-dialog__inner">
          <div className="search-dialog__header">
            <label htmlFor={`${listId}-input`}>Search the library</label>
            <button type="button" onClick={closePalette} aria-label="Close search">
              Esc
            </button>
          </div>

          <input
            id={`${listId}-input`}
            ref={inputRef}
            role="combobox"
            aria-autocomplete="list"
            aria-expanded={open}
            aria-controls={listId}
            aria-activedescendant={selected ? `${listId}-item-${activeIndex}` : undefined}
            aria-describedby={statusId}
            autoComplete="off"
            placeholder="Search titles, topics, descriptions…"
            value={query}
            onChange={(event) => {
              setQuery(event.target.value);
              setActiveIndex(0);
            }}
            onKeyDown={handleInputKeyDown}
          />

          <p id={statusId} className="search-dialog__status" aria-live="polite">
            {matches.length} {matches.length === 1 ? 'result' : 'results'}
            {!query.trim() ? ' suggested' : ''}
          </p>

          <div id={listId} role="listbox" aria-label="Search results"
               className="search-dialog__results">
            {matches.map((item, index) => (
              <div
                key={item.id}
                id={`${listId}-item-${index}`}
                role="option"
                aria-selected={index === activeIndex}
                className="search-result"
                onMouseEnter={() => setActiveIndex(index)}
                onClick={() => navigate(item.url)}
              >
                <span className="search-result__topic">{item.topic}</span>
                <strong>{item.title}</strong>
                <span className="search-result__description">{item.description}</span>
              </div>
            ))}
          </div>

          {matches.length === 0 && (
            <p className="search-dialog__empty">No matching resources. Try another keyword.</p>
          )}
          <p className="search-dialog__help">↑ ↓ Navigate · Enter Open · Esc Close</p>
        </div>
      </dialog>
    </>
  );
}
```

### Implementation notes
- `showModal()` makes the rest of the document inert and traps focus in the dialog.
- React's effect cleans up the global shortcut listener on unmount.
- The input retains DOM focus; `aria-activedescendant` communicates the active result.
- Clicking a result uses a normal full navigation. If you want Astro ClientRouter navigation, use its navigation API as a separate enhancement.
- The fallback anchor goes to `/resources` before hydration; after hydration the click handler opens the modal.
- This is a learning implementation: for production, add automated keyboard and screen-reader tests and verify how your selected React/Astro versions handle dialog focus after page swaps.

## 3. `src/styles/search-palette.css`

```css
@layer components {
  .search-trigger {
    display: inline-flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    min-inline-size: min(100%, 18rem);
    padding-block: .7rem;
    padding-inline: 1rem;
    border: 1px solid rgb(255 255 255 / .18);
    border-radius: .85rem;
    background: #222631;
    color: #f5f6fa;
    text-decoration: none;
  }

  .search-trigger kbd {
    font-size: .75rem;
    opacity: .7;
  }

  .search-dialog {
    inline-size: min(42rem, calc(100% - 1rem));
    max-block-size: min(85dvh, 42rem);
    margin: auto;
    padding: 0;
    border: 1px solid rgb(255 255 255 / .16);
    border-radius: clamp(.75rem, 2vw, 1.25rem);
    background: #171a23;
    color: #f6f7fb;
    box-shadow: 0 1.5rem 6rem rgb(0 0 0 / .35);
    overflow: auto;
    animation: palette-enter 180ms ease-out both;
  }

  .search-dialog::backdrop {
    background: rgb(0 0 0 / .7);
    backdrop-filter: blur(8px);
  }

  .search-dialog__inner {
    padding: clamp(.75rem, 3vw, 1.5rem);
  }

  .search-dialog__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    margin-block-end: .8rem;
  }

  .search-dialog__header button {
    color: inherit;
    background: transparent;
    border: 1px solid rgb(255 255 255 / .2);
    border-radius: .4rem;
    padding: .3rem .5rem;
    cursor: pointer;
  }

  .search-dialog input {
    inline-size: 100%;
    box-sizing: border-box;
    padding: .85rem 1rem;
    border-radius: .75rem;
    border: 1px solid rgb(255 255 255 / .25);
    background: #262b38;
    color: inherit;
    font: inherit;
  }

  .search-dialog__status,
  .search-dialog__help {
    font-size: .8rem;
    color: #b7bdca;
  }

  .search-dialog__results {
    display: grid;
    gap: .35rem;
    max-block-size: 45dvh;
    overflow-y: auto;
  }

  .search-result {
    display: grid;
    gap: .2rem;
    padding: .75rem;
    border-radius: .7rem;
    cursor: pointer;
    border: 1px solid transparent;
  }

  .search-result[aria-selected="true"] {
    background: #34394b;
    border-color: #a99cff;
  }

  .search-result__topic {
    color: #b8adff;
    font-size: .75rem;
  }

  .search-result__description {
    font-size: .85rem;
    color: #c3c7d2;
  }

  .search-dialog__empty {
    padding-block: 2rem;
    text-align: center;
  }

  .search-trigger:focus-visible,
  .search-dialog :is(button, input):focus-visible {
    outline: 2px solid #b9aeff;
    outline-offset: 3px;
  }

  @keyframes palette-enter {
    from { opacity: 0; transform: translateY(.6rem) scale(.99); }
    to { opacity: 1; transform: translateY(0) scale(1); }
  }

  @media (prefers-reduced-motion: reduce) {
    .search-dialog { animation: none; }
    .search-dialog::backdrop { backdrop-filter: none; }
  }
}
```

## 4. Integrate into the shared layout

```astro
---
import GlobalSearch from '../components/GlobalSearch.astro';
---

<header>
  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/resources">Resources</a>
    <GlobalSearch />
  </nav>
</header>
```

Keep your existing layout markup and imports; insert the component where the global search belongs.

## 5. Test plan

1. Run `npm run dev`, then `npm run build`.
2. Test ⌘K on Mac and Ctrl+K on Windows.
3. Test click, arrows, Enter, Escape and focus restoration.
4. Test empty search, no matches, and a real resource title.
5. Test at 320px and with a mobile virtual keyboard.
6. Disable JavaScript: the Search link must navigate to `/resources`.
7. With ClientRouter enabled, navigate across pages and repeat the shortcut.
8. Confirm no article bodies or private content are serialized into the index.

## Alternative approach
Use a build-generated `/search-index.json` file (similar to Task 08) and fetch it on the first open. This reduces the initial page payload for large libraries, but introduces loading/error states and an extra request.

## Stretch goal
Add grouped results by topic and a pure `searchResources()` function with unit tests for whitespace, casing, zero matches, and result limits.
