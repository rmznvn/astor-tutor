# Task 06 — Paginated Resource Archive

## Goal

Create a static archive that splits the resource collection into multiple pages.

Use a catch-all page route so the first page can live at:

~~~text
/resources/archive
~~~

and later pages at:

~~~text
/resources/archive/2
/resources/archive/3
~~~

## You will practice

- `getStaticPaths()`
- Astro's `paginate()`
- `Page<T>`
- catch-all dynamic routes
- Previous/Next URLs
- accessible pagination
- sorting before pagination

## Requirements

1. Add at least enough sample resources to create multiple pages.
2. Sort resources before paginating.
3. Show 6 items per page.
4. Render current page / total pages.
5. Add Previous and Next links.
6. Add numbered page links.
7. Mark the active page with `aria-current="page"`.
8. Do not render unusable links as clickable anchors.

## CSS practice

Use:

- intrinsic sizing
- `inline-size` / logical properties
- `:focus-visible`
- `:has()`
- CSS nesting if your project setup supports it
- `@starting-style` for a progressive card entrance animation

## Acceptance criteria

- Page 1 works without `/1`.
- Page 2+ use numeric URLs.
- Page order is deterministic.
- Current page is announced accessibly.
- Navigation is usable with keyboard only.
- `npm run build` generates every page.

## Hints

- `[...page].astro` is useful when the first page should omit the number.
- `paginate()` returns the route objects for you.
- The `page` prop includes data plus navigation metadata.

## Stretch goals

- Preserve a sort query parameter in pagination links.
- Add canonical URLs.
- Add a compact pager on mobile.
