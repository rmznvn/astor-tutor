# Task 09 — Production Metadata + RSS + Sitemap + Copy-Link UI

## Goal

Take the resource project from “works locally” to a more production-ready content site.

## You will practice

- reusable head metadata
- canonical URLs
- Open Graph metadata
- structured data
- Astro `site` configuration
- `@astrojs/sitemap`
- `@astrojs/rss`
- XML endpoint generation
- Clipboard API
- accessible toast feedback

## Requirements

1. Configure a production `site` URL in Astro config.
2. Create/reuse a `BaseHead.astro` component.
3. Every resource detail page must output:
   - title
   - meta description
   - canonical URL
   - Open Graph title/description/type
4. Add Article JSON-LD to detail pages.
5. Generate `sitemap.xml`.
6. Generate `rss.xml` from the resource collection.
7. Add a Copy Link button with visible confirmation.
8. Make the copy interaction work after Task 07 transitions.

## Content changes

Add a publication date to the resource schema.

Optionally add:

- updated date
- author
- social image

## CSS practice

Use:

- CSS cascade layers with `@layer`
- `color-mix()` in OKLab
- CSS custom properties for semantic colors
- logical positioning
- `:where()` for low-specificity defaults
- `prefers-color-scheme`
- subtle toast animation

## Acceptance criteria

- View Source contains metadata, not only client-injected tags.
- Canonical URLs are absolute.
- `/rss.xml` returns valid XML.
- build produces a sitemap.
- Copy Link works with keyboard.
- Success/error feedback is announced.
- No duplicate click handlers after client navigation.
- Build succeeds.

## Hints

- RSS requires a `site` to generate reliable absolute URLs.
- Sitemap is an Astro integration.
- Structured data is JSON rendered into a script tag, not visible UI.
- Keep copy-link JS small and framework-free.

## Stretch goals

- Add an RSS autodiscovery `<link>`.
- Add dynamic social images later.
- Add BreadcrumbList JSON-LD.
- Add “last updated” metadata to articles.
