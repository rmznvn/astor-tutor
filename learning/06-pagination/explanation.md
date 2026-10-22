# Task 06 — Explanation

## Why paginate at build time?

For a static content archive, Astro can create each page ahead of time.

~~~text
all resources
    ↓ sort
paginate(..., { pageSize: 6 })
    ↓
archive/
archive/2/
archive/3/
~~~

There is no need to ship JavaScript just to divide a static collection into pages.

## Why sort first?

Pagination depends on order.

If the order changes unpredictably, items can move between pages between builds. Always create a stable ordering before calling `paginate()`.

## What is the page prop?

The generated page object contains:

- `data`
- current page number
- total pages
- previous URL
- next URL
- range information

This is more useful than manually slicing arrays.

## Interview questions

### Pagination or infinite scroll?

A strong answer:

Pagination is easier to link, index, navigate with keyboard, and restore. Infinite scroll can be useful for discovery-heavy interfaces but requires careful focus, history, and accessibility handling.

### Why use a catch-all route here?

A strong answer:

A catch-all pagination route allows the first page to resolve at the archive root while later pages receive numeric segments.

### Should filtering happen before or after pagination?

A strong answer:

It depends on product behavior, but users usually expect the full filtered result set to be computed first and then paginated. Client-only filtering of one already-paginated page can be misleading.
