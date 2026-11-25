# Task 31 — Markdown External Link Policy

## Reference implementation
`plugins/remark-external-links.mjs`
```js
export default function externalLinks({ site }) {
  const origin = new URL(site).origin;
  return function transform(tree) {
    function walk(node) {
      if (node.type === 'link' && typeof node.url === 'string') {
        try {
          // Only explicit web URLs or protocol-relative URLs are candidates.
          if (/^(https?:\/\/|\/\/)/i.test(node.url)) {
            const url = new URL(node.url, origin);
            if (url.origin !== origin && !node.data?.hProperties?.['data-external']) {
              node.data ??= {};
              node.data.hProperties ??= {};
              node.data.hProperties['data-external'] = 'true';
              node.children.push({ type: 'text', value: ' (external)' });
            }
          }
        } catch { /* Leave malformed URLs unchanged for the content audit. */ }
      }
      for (const child of node.children ?? []) walk(child);
    }
    walk(tree);
  };
}
```

Merge into `astro.config.mjs` (preserve existing integrations and options):
```js
import { defineConfig } from 'astro/config';
import externalLinks from './plugins/remark-external-links.mjs';
export default defineConfig({
  site: 'https://example.com',
  markdown: { remarkPlugins: [[externalLinks, { site: 'https://example.com' }]] }
});
```
Replace example.com with your actual production origin in both places.

`src/pages/link-policy.md`
```md
---
title: Link policy
---
# Link policy

[Internal](/releases)

[Same origin](https://example.com/releases)

[Astro docs](https://docs.astro.build/)

[Email](mailto:hello@example.com)
```

Add to the global stylesheet and ensure the shared layout imports it; for the standalone Markdown fixture, use the existing Markdown layout or create the following file and add `layout: ../layouts/LinkPolicy.astro` to its frontmatter.

`src/layouts/LinkPolicy.astro`
```astro
---
import '../styles/global.css';
---
<html lang="en"><head><meta charset="utf-8"/><meta name="viewport" content="width=device-width"/><title>Link policy</title></head><body><main><slot /></main></body></html>
```

```css
a[data-external] { text-decoration-thickness: .12em; text-underline-offset: .2em; }
a[data-external]::after { content: ""; display: inline-block; inline-size: .55em; block-size: .55em; margin-inline-start: .3em; border-block-start: 2px solid currentColor; border-inline-end: 2px solid currentColor; }
a:focus-visible { outline: 2px solid currentColor; outline-offset: 3px; }
```
The empty pseudo-element is decorative; the text suffix carries meaning. Inspect generated HTML after build. This plugin covers inline Markdown links, not reference-style links or raw HTML; extending those is a separate exercise.

## Verification
Run npm run build and npm run preview. External HTTPS and HTTP links get exactly one suffix; same-origin and relative links do not; existing link text remains; no target=_blank added; keyboard focus visible; no client script..

## Alternative
Use a rehype plugin if you need to transform generated HTML links as well as Markdown link nodes.

This reference has not been executed in the repository.
