# Task 32 — Custom Astro Integration & Build Metrics

## Reference implementation
`integrations/html-metrics.mjs`
```js
import { readdir, stat, writeFile } from 'node:fs/promises';
import { join, relative } from 'node:path';
import { fileURLToPath } from 'node:url';
export default function htmlMetrics() {
  return {
    name: 'html-metrics',
    hooks: {
      'astro:build:done': async ({ dir }) => {
        const root = fileURLToPath(dir);
        const rows = [];
        async function walk(folder) {
          for (const item of await readdir(folder, { withFileTypes: true })) {
            const path = join(folder, item.name);
            if (item.isDirectory()) await walk(path);
            else if (item.isFile() && item.name.endsWith('.html')) {
              rows.push({ path: relative(root, path).replaceAll('\\', '/'), bytes: (await stat(path)).size });
            }
          }
        }
        await walk(root);
        rows.sort((a,b) => b.bytes-a.bytes || a.path.localeCompare(b.path));
        await writeFile('html-metrics.json', JSON.stringify(rows, null, 2));
      }
    }
  };
}
```
Add `html-metrics.json` to .gitignore. Run builds from the project root. This exercise assumes static output; SSR bundles require a different measurement.

Merge into existing `astro.config.mjs`:
```js
import { defineConfig } from 'astro/config';
import htmlMetrics from './integrations/html-metrics.mjs';
export default defineConfig({ integrations: [htmlMetrics()] });
```

`src/pages/build-report.astro`
```astro
<html lang="en">
<head><meta charset="utf-8"/><meta name="viewport" content="width=device-width"/><title>Build report</title></head>
<body><main>
<h1>HTML size report</h1>
<p>Budget: 100,000 raw bytes per page. Select the report from your local build.</p>
<label for="report">Report JSON</label>
<input id="report" type="file" accept=".json,application/json"/>
<p id="status" role="status"></p>
<ul id="rows"></ul>
</main></body></html>
<script>
const input = document.querySelector<HTMLInputElement>('#report');
const status = document.querySelector('#status');
const list = document.querySelector('#rows');
let generation = 0;
input?.addEventListener('change', async () => {
  const current = ++generation;
  list?.replaceChildren();
  if (status) status.textContent = '';
  const file = input.files?.[0];
  if (!file || !list || !status) return;
  try {
    if (file.size > 2_000_000) throw new Error();
    const data: unknown = JSON.parse(await file.text());
    if (current !== generation) return;
    if (!Array.isArray(data) || data.length > 5000 || !data.every(r =>
      r && typeof r.path === 'string' && Number.isSafeInteger(r.bytes) && r.bytes >= 0
    )) throw new Error();
    const fragment = document.createDocumentFragment();
    for (const row of [...data].sort((a,b) => b.bytes-a.bytes)) {
      const li = document.createElement('li');
      const label = document.createElement('span');
      label.textContent = row.path + ': ' + row.bytes + ' bytes' + (row.bytes > 100000 ? ' (over budget)' : '');
      const meter = document.createElement('meter');
      meter.max = Math.max(100000, row.bytes);
      meter.high = 100000;
      meter.value = row.bytes;
      meter.setAttribute('aria-label', row.path + ' raw bytes');
      li.append(label, meter);
      fragment.append(li);
    }
    list.append(fragment);
    status.textContent = data.length + ' pages loaded.';
  } catch {
    if (current === generation) status.textContent = 'Invalid or unreadable report.';
  }
});
</script>
<style>
main { max-inline-size: 70rem; margin-inline: auto; padding: 1rem; }
ul { padding: 0; list-style: none; }
li { display: grid; grid-template-columns: minmax(0, 2fr) minmax(0, 1fr); gap: 1rem; padding-block: .5rem; overflow-wrap: anywhere; }
meter { inline-size: 100%; align-self: center; }
@media (max-width: 35rem) { li { grid-template-columns: 1fr; } }
</style>
```
Use this standalone route with full-page navigation; if integrating ClientRouter, move initialization into an idempotent astro:page-load handler. Run npm run build, then npm run preview and open /build-report. Select the generated JSON; try a malformed file and a path containing HTML markup.

## Verification
Run npm run build and npm run preview. Build emits report at project root; paths are relative; report is sorted; valid file displays sizes; malformed file gives a status message; HTML-like paths render as text; no network request or external dependency..

## Alternative
A standalone postbuild Node script is easier to reuse outside Astro; an integration is preferable when you want access to Astro lifecycle data.

This reference has not been executed in the repository.
