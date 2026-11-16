# Task 20 — Reference implementation

Try `task.md` first. This solution adds one Astro component to the **existing** resource page; it does not create a separate app. Replace the sample mapping with your existing content collection's actual fields.

## 1. Component: `src/components/ReadingList.astro`

```astro
---
interface Resource {
  id: string;
  title: string;
  href: string;
}
interface Props {
  resources: Resource[];
}
const { resources } = Astro.props;
// The page passes its collection-derived resource catalog.
// The browser receives only these public IDs, titles, and paths.
---

<section class="reading-list" data-reading-list aria-labelledby="reading-list-title">
  <header class="reading-list__header">
    <div>
      <h2 id="reading-list-title">Read later</h2>
      <p>Save up to 20 resources and share your selection.</p>
    </div>
    <output data-saved-count aria-live="polite">0 saved</output>
  </header>

  <div class="reading-list__cards">
    {resources.map((resource) => (
      <article class="reading-list__card">
        <a href={resource.href}>{resource.title}</a>
        <button type="button" data-save-id={resource.id} aria-pressed="false">
          Save
        </button>
      </article>
    ))}
  </div>

  <aside class="reading-list__tray" aria-label="Saved resources">
    <h3>My selection</h3>
    <p data-saved-empty>No resources selected yet.</p>
    <ul data-saved-items></ul>
    <button type="button" data-copy-list>Copy share link</button>
    <p data-save-status role="status" aria-live="polite"></p>
    <p data-copy-fallback hidden>
      Copy this link manually: <a data-manual-url href=""></a>
    </p>
  </aside>

  <!-- Safe JSON serialization by Astro; the script parses textContent, not HTML. -->
  <script type="application/json" data-resource-catalog set:html={JSON.stringify(resources).replace(/</g, '\\u003c')} />
</section>

<script>
  import './reading-list.ts';
</script>

<style>
  .reading-list {
    --line: color-mix(in oklab, currentColor 18%, transparent);
    display: grid;
    gap: clamp(1rem, 2vw, 1.5rem);
    padding-block: 1rem 2rem;
  }
  .reading-list__header {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    gap: 1rem;
  }
  .reading-list__cards { display: grid; gap: .75rem; }
  .reading-list__card {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 1rem;
    border: 1px solid var(--line);
    border-radius: .75rem;
    padding: 1rem;
  }
  .reading-list__tray {
    container-type: inline-size;
    border: 1px solid var(--line);
    border-radius: 1rem;
    padding: clamp(1rem, 3vw, 1.5rem);
    animation: tray-in 250ms ease-out both;
  }
  .reading-list__tray ul {
    display: grid;
    gap: .5rem;
    padding-inline-start: 1.25rem;
  }
  @container (min-width: 32rem) {
    .reading-list__tray ul { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  }
  .reading-list button { cursor: pointer; }
  .reading-list :focus-visible { outline: 2px solid currentColor; outline-offset: 3px; }
  @keyframes tray-in {
    from { opacity: .65; transform: translateY(6px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @media (prefers-reduced-motion: reduce) {
    .reading-list__tray { animation: none; }
  }
  @media (max-width: 25rem) {
    .reading-list__card { align-items: flex-start; flex-direction: column; }
  }
</style>
```

## 2. Browser controller: `src/components/reading-list.ts`

```ts
type Resource = { id: string; title: string; href: string };
const STORAGE_KEY = 'frontend-hub:reading-list:v1';
const LIMIT = 20;
let activeController: AbortController | undefined;

function readStorage(): string[] {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    const parsed: unknown = raw ? JSON.parse(raw) : [];
    return Array.isArray(parsed) ? parsed.filter((id): id is string => typeof id === 'string') : [];
  } catch {
    return []; // Private browsing or blocked storage must not crash UI.
  }
}

function writeStorage(ids: string[]) {
  try { localStorage.setItem(STORAGE_KEY, JSON.stringify(ids)); }
  catch { /* In-memory selection still works in this visit. */ }
}

function normalize(ids: string[], allowed: Set<string>): string[] {
  return [...new Set(ids.filter(id => allowed.has(id)))].slice(0, LIMIT);
}

function fromUrl(allowed: Set<string>): string[] | null {
  const params = new URL(location.href).searchParams;
  if (!params.has('saved')) return null; // Explicit ?saved= means empty.
  return normalize((params.get('saved') ?? '').split(','), allowed);
}

function shareUrl(ids: string[]): string {
  const url = new URL(location.href);
  url.searchParams.set('saved', ids.join(','));
  return url.href; // Preserve unrelated query parameters and the hash.
}

function initReadingList() {
  activeController?.abort(); // Avoid duplicate handlers after ClientRouter navigation.
  activeController = new AbortController();
  const signal = activeController.signal;
  const root = document.querySelector<HTMLElement>('[data-reading-list]');
  if (!root) return;

  const catalogNode = root.querySelector<HTMLScriptElement>('[data-resource-catalog]');
  let resources: Resource[] = [];
  try { resources = JSON.parse(catalogNode?.textContent ?? '[]'); }
  catch { return; }

  const catalog = new Map(resources.map(item => [item.id, item]));
  const allowed = new Set(catalog.keys());
  let selected = fromUrl(allowed) ?? normalize(readStorage(), allowed);
  const count = root.querySelector<HTMLOutputElement>('[data-saved-count]')!;
  const items = root.querySelector<HTMLUListElement>('[data-saved-items]')!;
  const empty = root.querySelector<HTMLElement>('[data-saved-empty]')!;
  const status = root.querySelector<HTMLElement>('[data-save-status]')!;
  const fallback = root.querySelector<HTMLElement>('[data-copy-fallback]')!;
  const manual = root.querySelector<HTMLAnchorElement>('[data-manual-url]')!;

  function render() {
    const chosen = new Set(selected);
    count.textContent = `${selected.length} saved`;
    empty.hidden = selected.length > 0;
    items.replaceChildren(); // No untrusted innerHTML.
    for (const id of selected) {
      const item = catalog.get(id);
      if (!item) continue;
      const li = document.createElement('li');
      const a = document.createElement('a');
      a.href = item.href;
      a.textContent = item.title;
      li.append(a);
      items.append(li);
    }
    root.querySelectorAll<HTMLButtonElement>('[data-save-id]').forEach(button => {
      const saved = chosen.has(button.dataset.saveId ?? '');
      button.setAttribute('aria-pressed', String(saved));
      button.textContent = saved ? 'Remove' : 'Save';
    });
  }

  root.addEventListener('click', (event) => {
    const target = event.target;
    if (!(target instanceof Element)) return;
    const button = target.closest<HTMLButtonElement>('[data-save-id]');
    if (!button || !root.contains(button)) return;
    const id = button.dataset.saveId;
    if (!id || !allowed.has(id)) return;
    if (selected.includes(id)) selected = selected.filter(value => value !== id);
    else if (selected.length < LIMIT) selected = [...selected, id];
    else { status.textContent = 'You can save up to 20 resources.'; return; }
    writeStorage(selected); // Only user edits persist; opening shared URL does not.
    history.replaceState(history.state, '', shareUrl(selected));
    status.textContent = 'Reading list updated.';
    fallback.hidden = true;
    render();
  }, { signal });

  root.querySelector<HTMLButtonElement>('[data-copy-list]')?.addEventListener('click', async () => {
    const url = shareUrl(selected);
    try {
      if (!navigator.clipboard?.writeText) throw new Error('Clipboard unavailable');
      await navigator.clipboard.writeText(url);
      status.textContent = 'Share link copied.';
      fallback.hidden = true;
    } catch {
      status.textContent = 'Could not copy automatically.';
      manual.href = url;
      manual.textContent = url;
      fallback.hidden = false;
    }
  }, { signal });

  window.addEventListener('popstate', () => {
    selected = fromUrl(allowed) ?? normalize(readStorage(), allowed);
    render();
  }, { signal });

  render();
}

// The module can execute on the initial visit or be reused by ClientRouter.
document.addEventListener('astro:page-load', initReadingList);
if (document.readyState !== 'loading') initReadingList();
else document.addEventListener('DOMContentLoaded', initReadingList, { once: true });
```

## 3. Integrate into the existing resource page

Reuse the collection and its current route mapping. Example (adapt `title`, `slug`, and route prefix to your existing schema):

```astro
---
import { getCollection } from 'astro:content';
import ReadingList from '../components/ReadingList.astro';

const entries = await getCollection('resources');
const resources = entries.map(entry => ({
  id: entry.id,
  title: entry.data.title,
  href: `/resources/${entry.id}/`,
}));
---
<ReadingList resources={resources} />
```

If your collection is called something else, use its existing name. If a resource has a slug distinct from its ID, use the existing detail-page URL generator. Do not create duplicate routes.

## 4. Manual verification
1. Start with no `saved` query: Save two cards, reload, verify both remain.
2. Open `?saved=`: verify empty state even if localStorage has saved IDs.
3. Open `?saved=unknown,known,known`: only the known ID should appear once.
4. Save while `?topic=astro#resources` is present: topic and hash remain.
5. Copy link, open in another browser/profile: shared list appears without overwriting personal storage.
6. Disable storage and clipboard permissions: buttons still work and fallback URL is shown.
7. Navigate away/back using ClientRouter: one click changes state once.
8. Test 320px width, keyboard tab/focus, and reduced-motion setting.
9. Run `npm run build`.

## Alternative
For complex multi-device lists, store IDs in a user account with an authenticated Astro Action (Task 13) and render a small server island (Task 16). For this exercise, the browser-only solution is faster, cheaper, and requires no server.
