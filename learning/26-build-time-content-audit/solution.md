# Task 26 — Reference solution

The `resources` collection and `imageAlt`/`relatedSlugs` fields are examples; map them to your actual content schema.

```ts
// src/lib/auditContent.ts
export type EntryMeta = {
  slug: string; source: string; description?: string;
  hasImage?: boolean; imageAlt?: string; relatedSlugs?: string[];
};
export type Finding = { severity: 'error' | 'warning'; source: string; message: string };
export function auditContent(entries: EntryMeta[]): Finding[] {
  const findings: Finding[] = [];
  const slugs = new Map<string, string>();
  for (const entry of entries) {
    const slug = entry.slug.trim();
    if (!slug) findings.push({severity:'error',source:entry.source,message:'Missing slug'});
    else if (slugs.has(slug)) findings.push({severity:'error',source:entry.source,
      message:`Duplicate slug ${slug} (also in ${slugs.get(slug)})`});
    else slugs.set(slug, entry.source);
    if (!entry.description?.trim())
      findings.push({severity:'warning',source:entry.source,message:'Missing description'});
    if (entry.hasImage && !entry.imageAlt?.trim())
      findings.push({severity:'error',source:entry.source,message:'Missing image alt text'});
  }
  for (const entry of entries) for (const related of entry.relatedSlugs ?? [])
    if (!slugs.has(related))
      findings.push({severity:'error',source:entry.source,message:`Unknown related slug ${related}`});
  return findings;
}

// src/pages/audit.json.ts (static endpoint)
import type { APIRoute } from 'astro';
import { getCollection } from 'astro:content';
import { auditContent } from '../lib/auditContent';
export const prerender = true;
export const GET: APIRoute = async () => {
  const entries = await getCollection('resources');
  const findings = auditContent(entries.map(entry => ({
    slug: entry.slug, source: entry.id,
    description: entry.data.description,
    hasImage: Boolean(entry.data.image),
    imageAlt: entry.data.imageAlt,
    relatedSlugs: entry.data.relatedSlugs ?? [],
  })));
  return new Response(JSON.stringify({findings}, null, 2),
    {headers:{'Content-Type':'application/json; charset=utf-8'}});
};

// scripts/check-content.mjs (plain Node; do not import astro:content)
import { readFile } from 'node:fs/promises';
const { findings } = JSON.parse(await readFile('dist/audit.json', 'utf8'));
for (const finding of findings) {
  const line = `[${finding.severity}] ${finding.source}: ${finding.message}`;
  (finding.severity === 'error' ? console.error : console.warn)(line);
}
if (findings.some(x => x.severity === 'error')) process.exitCode = 1;

// package.json script:
// "audit:content": "astro build && node scripts/check-content.mjs"
```

```ts
// src/lib/auditContent.test.ts
import { it, expect } from 'vitest';
import { auditContent } from './auditContent';
it('rejects duplicate slugs', () => {
  const rows = [{slug:'a',source:'1',description:'ok'},
                {slug:'a',source:'2',description:'ok'}];
  expect(auditContent(rows).some(x => x.message.includes('Duplicate'))).toBe(true);
});
it('rejects missing references', () => {
  expect(auditContent([{slug:'a',source:'1',description:'ok',relatedSlugs:['x']}])
    .some(x => x.severity === 'error')).toBe(true);
});
it('accepts valid content', () => {
  expect(auditContent([{slug:'a',source:'1',description:'ok'}])).toEqual([]);
});
```

```css
.audit-checklist { container-type: inline-size; }
.audit-checklist label { display: flex; gap: .75rem; padding: .75rem; }
.audit-checklist label:has(input:checked) { text-decoration: line-through; opacity: .7; }
.audit-checklist input:focus-visible { outline: 2px solid currentColor; outline-offset: 3px; }
@container (min-width: 40rem) { .audit-checklist .items { display: grid; grid-template-columns: repeat(2, 1fr); } }
```

**Important:** The static audit file is publicly generated. Avoid putting private editorial information into it, or write the report via a build integration instead. For SSR builds, do not assume a dynamic endpoint is emitted to `dist`. Verify your adapter's output.
