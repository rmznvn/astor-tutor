# Task 09 — Explanation

## SEO is page architecture, not a plugin

A content page should be understandable before client JavaScript runs.

That means important metadata should be rendered by Astro into the document head.

## Canonical URL

A canonical URL tells search engines which absolute URL represents the preferred version of a page.

It is especially useful if tracking/query parameters can produce multiple URLs for the same content.

## Sitemap vs RSS

They solve different problems.

### Sitemap

Primarily helps crawlers discover site URLs.

### RSS

Provides a subscription feed of content items for feed readers and other consumers.

## Structured data

JSON-LD adds machine-readable context.

For a resource article, it can describe:

- headline
- date published
- date modified
- canonical URL
- author

It does not replace visible semantic HTML.

## Why keep share interaction framework-free?

A copy button needs very little state:

~~~text
click
→ clipboard.writeText()
→ show message
~~~

Shipping a React island for this single action would usually be unnecessary.

## Interview questions

### Why set `site` in Astro config?

A strong answer:

Astro and integrations need the production origin to generate absolute canonical, sitemap, and feed URLs reliably.

### Does a sitemap improve ranking directly?

A strong answer:

Not by itself. It helps search engines discover and understand crawlable URLs, especially on larger sites.

### Why render metadata server-side/build-time?

A strong answer:

It is immediately available in the HTML response to crawlers, link preview services, and users even before client JavaScript executes.
