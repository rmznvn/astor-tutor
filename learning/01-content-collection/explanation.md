# Task 01 — Explanation

## Mental model

Think of an Astro Content Collection as a structured content source.

~~~text
Markdown files
      ↓
Content Collection
      ↓
getCollection()
      ↓
array of entries
      ↓
.map()
      ↓
ResourceCard.astro
~~~

`getCollection()` returns entries. Your custom fields live inside `entry.data`.

Example:

~~~js
{
  id: "astro-routing",
  data: {
    title: "Understanding Astro Routing",
    topic: "Astro",
    readingTime: 7
  }
}
~~~

So you read:

~~~js
resource.data.title
~~~

not:

~~~js
resource.title
~~~

## Why split the components?

The page should coordinate data and page structure.

`ResourcesGrid.astro` owns the list.

`ResourceCard.astro` owns the markup for one resource.

That makes the card reusable later on:

- Home page
- Resources page
- Related content
- Search results

## Why Content Collections instead of a JSON import?

A JSON array is fine for simple data.

Content Collections become more useful when your content grows because they give you:

- a consistent content API
- loaders
- schemas/type safety
- Markdown/MDX support
- easier migration to CMS-driven content

## Why container queries?

A viewport media query asks:

> How wide is the browser?

A container query asks:

> How much space does this component actually have?

That is useful when the same card can appear in different layouts.

## Interview questions

### Why is `ResourceCard.astro` separate from the page?

A strong answer:

The page handles data loading and composition, while the card encapsulates repeated presentation. This avoids duplicated markup and makes the card reusable.

### Why use `getCollection()`?

A strong answer:

It gives Astro a structured way to retrieve all entries from a named content collection and works consistently across different content loaders.

### Why avoid JavaScript for the hover animation?

A strong answer:

The interaction is purely visual, so CSS is simpler, cheaper, and avoids unnecessary client-side JavaScript.
