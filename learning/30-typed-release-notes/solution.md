# Task 30 — Typed Release Notes & Native Disclosure

## Reference implementation
Install no extra dependencies on current Astro versions supporting the Content Layer. If a content config already exists, merge this collection into its exports rather than replacing it.

`src/content.config.ts`
```ts
import { defineCollection, z } from 'astro:content';
import { glob } from 'astro/loaders';
const releases = defineCollection({
  loader: glob({ base: './src/content/releases', pattern: '**/*.md' }),
  schema: z.object({
    title: z.string().min(1),
    date: z.coerce.date(),
    category: z.enum(['Feature', 'Fix']),
  }),
});
export const collections = { releases };
```

`src/content/releases/search.md`
```md
---
title: Better search
date: 2026-11-20
category: Feature
---
Search now supports resource titles.
```

`src/content/releases/focus.md`
```md
---
title: Keyboard focus fix
date: 2026-11-18
category: Fix
---
Focus is now visible on navigation links.
```

`src/pages/releases.astro`
```astro
---
import { getCollection, render } from 'astro:content';
const entries = (await getCollection('releases')).sort(
  (a,b) => b.data.date.getTime()-a.data.date.getTime() || a.id.localeCompare(b.id)
);
const rows = await Promise.all(entries.map(async entry => ({
  entry, Content: (await render(entry)).Content
})));
---
<html lang="en">
<head><meta charset="utf-8"/><meta name="viewport" content="width=device-width"/><title>Release notes</title></head>
<body><main>
<h1>Release notes</h1>
{rows.length === 0 ? <p>No releases yet.</p> : <ol>
{rows.map(({entry,Content}) => <li>
  <time datetime={entry.data.date.toISOString()}>{entry.data.date.toISOString().slice(0,10)}</time>
  <details><summary>{entry.data.title} — {entry.data.category}</summary><Content /></details>
</li>)}
</ol>}
</main></body></html>
<style>
main { max-inline-size: 65rem; margin-inline: auto; padding: 1rem; }
ol { list-style: none; padding: 0; }
li { display: grid; grid-template-columns: minmax(0, 7rem) minmax(0, 1fr); gap: 1rem; padding-block: 1rem; border-inline-start: 2px solid currentColor; padding-inline-start: .75rem; overflow-wrap: anywhere; }
summary { cursor: pointer; }
summary:focus-visible { outline: 2px solid currentColor; outline-offset: 4px; }
@media (max-width: 30rem) { li { grid-template-columns: 1fr; } }
</style>
```
If your existing project has a shared layout, replace the HTML shell with it. Link /releases from the existing navigation.

## Verification
Run npm run build and npm run preview. Two sample releases appear newest first; keyboard toggles details; invalid category fails validation; empty collection shows a message; no new browser JS; no horizontal overflow at 320px..

## Alternative
Use a JSON collection for short plain-text updates; Markdown is preferable when editors need formatted release notes.

This reference has not been executed in the repository.
