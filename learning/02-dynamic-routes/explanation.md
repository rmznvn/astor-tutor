# Task 02 — Explanation

## Dynamic route mental model

A file named:

~~~text
src/pages/resources/[slug].astro
~~~

is a template for multiple URLs.

For SSG, Astro asks:

> Which values of `slug` should exist at build time?

That is what `getStaticPaths()` answers.

~~~text
collection entries
      ↓
getStaticPaths()
      ↓
slug list
      ↓
Astro builds HTML
      ↓
/resources/astro-routing
/resources/array-methods
...
~~~

## Why pass the entry in props?

You could generate a slug and query the item again inside the page.

But passing the entry through `props` avoids repeating work and makes the route template easier to read.

## Why not React for the article?

The article itself is static content.

Only two pieces need browser state:

- scroll progress
- currently visible heading

A small script is lighter than hydrating a React component for the whole article.

## Why `IntersectionObserver`?

It is designed for visibility changes.

Instead of running expensive heading calculations on every scroll event, the browser can notify you when headings cross configured thresholds.

## Interview questions

### Why does SSG need `getStaticPaths()`?

Because Astro must enumerate all concrete dynamic URLs before producing static HTML files.

### What changes with SSR?

With SSR, the server can read the route parameter at request time, so every possible slug does not need to be generated ahead of time.

### Why is `scroll-margin-top` useful?

It prevents anchored headings from being hidden underneath sticky headers or navigation after clicking a TOC link.
