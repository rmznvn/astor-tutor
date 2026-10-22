# Task 08 — Explanation

## What is an Astro endpoint?

Pages usually return HTML.

An endpoint can return another HTTP response:

~~~text
JSON
XML
text
images
other generated files
~~~

For this static project, the JSON endpoint can be generated during build from the same Content Collection used by your pages.

## Why select fields?

This is better:

~~~js
{
  id,
  title,
  topic,
  url
}
~~~

than blindly doing:

~~~js
JSON.stringify(resource)
~~~

Public endpoints should intentionally define their contract.

That improves:

- payload size
- stability
- privacy
- maintainability

## Client/server boundary

The collection query runs in Astro's build/server context.

The random selection UI runs in the browser.

~~~text
Content Collection
      ↓
Astro endpoint
      ↓ JSON
browser fetch()
      ↓
dialog UI
~~~

## Interview questions

### Why create an endpoint if Astro already has the collection?

A strong answer:

Server-rendered pages can access the collection directly, but browser JavaScript cannot import server-only content APIs. An endpoint creates an explicit HTTP boundary when client code needs data.

### Static or server endpoint?

A strong answer:

If the data only changes when the site rebuilds, a static endpoint is simple and cacheable. If it must reflect request-time data, authentication, or rapidly changing content, use on-demand/server rendering.

### Why use dialog instead of building a div modal?

A strong answer:

The native dialog element provides modal semantics, focus behavior, Escape handling, and a backdrop, reducing the amount of accessibility behavior we need to recreate.
