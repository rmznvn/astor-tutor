# Task 02 — Dynamic Resource Pages

## Goal

Turn the resources from Task 01 into real detail pages.

Example URLs:

~~~text
/resources/astro-routing
/resources/css-container-queries
/resources/array-methods
~~~

## You will practice

- file-based routing
- `[slug].astro`
- `getStaticPaths()`
- `Astro.props`
- collection rendering
- layouts
- sticky UI
- small client-side scripts without React

## Requirements

Create:

~~~text
src/pages/resources/index.astro
src/pages/resources/[slug].astro
~~~

The detail page must include:

- back link
- metadata
- title and description
- rendered article body
- table of contents
- reading progress
- Previous / Next navigation

## CSS practice

Use:

- custom properties
- `position: sticky`
- `scroll-margin-top`
- `text-wrap: balance`
- `:has()`
- pseudo-elements
- responsive grid

## Interaction

Use small browser-side JavaScript for:

- reading progress
- active TOC item

Do not convert the article to React.

## Acceptance criteria

- Every resource gets a detail URL.
- URLs are generated through the dynamic route.
- Content body renders correctly.
- TOC links work.
- Active section can update while scrolling.
- Reading progress works.
- Previous/Next links are generated from collection order.
- `npm run build` succeeds.

## Hints

1. For SSG, Astro needs to know every slug at build time.
2. `getStaticPaths()` can return both `params` and `props`.
3. `render(entry)` gives you a renderable `Content` component.
4. Use `IntersectionObserver` for active headings.
5. Keep the browser script tiny.

## Stretch goals

- Copy-link buttons for headings.
- Automatic reading time.
- Breadcrumb JSON-LD.
- Smooth but accessible active-TOC animation.
