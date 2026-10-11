# Task 01 — Reusable Content Section

## Goal

Build a `Latest Resources` section powered by an Astro Content Collection.

The page should render resource cards from content files instead of hard-coded HTML.

## You will practice

- `content.config.ts`
- `defineCollection()`
- `glob()`
- `getCollection()`
- Astro component props
- mapping collection entries into reusable components
- conditional rendering
- separating content, presentation, and page composition

## Required structure

Create:

~~~text
src/
├── content/
│   └── resources/
│       ├── astro-routing.md
│       ├── css-container-queries.md
│       └── array-methods.md
├── components/
│   ├── ResourceCard.astro
│   └── ResourcesGrid.astro
└── content.config.ts
~~~

Each resource should include at least:

- title
- description
- topic
- reading time
- featured boolean

## UI requirements

Render a responsive grid of cards. Every card should show topic, title, description, and reading time.

A featured resource should have a distinct visual treatment without requiring a second card component.

## CSS practice

Use these techniques in this task:

- CSS Grid
- `repeat(auto-fit, minmax(...))`
- `clamp()`
- `aspect-ratio`
- container queries
- pseudo-element for a decorative gradient

## Interaction

On hover/focus:

- card lifts slightly
- border/gradient becomes more visible
- arrow moves a few pixels
- media area scales subtly

Respect `prefers-reduced-motion`.

## Acceptance criteria

- No resource card is hard-coded in `index.astro`.
- The collection is read with `getCollection()`.
- `ResourceCard.astro` receives data through props.
- Adding a fourth markdown file creates a fourth card without changing the page template.
- Keyboard focus is visible.
- Layout works from mobile to desktop.
- `npm run build` succeeds.

## Hints

1. First configure the collection before building the UI.
2. Log the result of `getCollection('resources')` once so you can inspect the entry shape.
3. Keep the card dumb: pass it one resource and let it render.
4. Use a wrapper with `container-type: inline-size` if you want the card layout to react to its own available width.

## Explanation

`getCollection()` returns entries, while the content fields live under each entry's `data` object. Your page or grid component owns the list; the card owns the markup for one item.

This separation matters because the same card could later be rendered on a homepage, search page, or related-content section without duplicating markup.

### Interview question

**Why use a Content Collection instead of importing a JSON array directly?**

A strong answer: Content Collections give Astro a structured content layer with loaders, optional schemas/type safety, and a consistent API such as `getCollection()`. They scale better when content becomes markdown, MDX, CMS data, or multiple sources.

### Interview question

**Why is `ResourceCard.astro` a component instead of keeping everything in the page?**

A strong answer: the page should coordinate data and page structure, while the card encapsulates repeated UI. That reduces duplication and makes the component reusable.

<details>
<summary><strong>Reference solution — open after your attempt</strong></summary>

### `src/content.config.ts`

~~~ts
import { defineCollection } from 'astro:content';
import { glob } from 'astro/loaders';

const resources = defineCollection({
  loader: glob({
    base: './src/content/resources',
    pattern: '**/*.md',
  }),
});

export const collections = { resources };
~~~

### Example resource

~~~md
---
title: "Understanding Astro Routing"
description: "Learn how Astro maps files to URLs and when to use dynamic routes."
topic: "Astro"
readingTime: 7
featured: true
---
~~~

### `src/components/ResourceCard.astro`

~~~astro
---
const { resource } = Astro.props;
const { title, description, topic, readingTime, featured } = resource.data;
---

<article class:list={['resource-card', { 'is-featured': featured }]}>
  <div class="resource-card__media" aria-hidden="true"></div>
  <div class="resource-card__content">
    <p class="resource-card__topic">{topic}</p>
    <h2>{title}</h2>
    <p>{description}</p>
    <div class="resource-card__meta">
      <span>{readingTime} min read</span>
      <span class="resource-card__arrow">→</span>
    </div>
  </div>
</article>
~~~

### `src/components/ResourcesGrid.astro`

~~~astro
---
import ResourceCard from './ResourceCard.astro';
const { resources } = Astro.props;
---

<section class="resources">
  <div class="resources__grid">
    {resources.map((resource) => <ResourceCard resource={resource} />)}
  </div>
</section>
~~~

### Page query

~~~astro
---
import { getCollection } from 'astro:content';
import ResourcesGrid from '../components/ResourcesGrid.astro';

const resources = await getCollection('resources');
---

<ResourcesGrid resources={resources} />
~~~

### CSS direction

~~~css
.resources__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 18rem), 1fr));
  gap: clamp(1rem, 2vw, 1.5rem);
}

.resource-card {
  container-type: inline-size;
  position: relative;
  overflow: hidden;
  border: 1px solid rgb(255 255 255 / 0.12);
  border-radius: 1.25rem;
  transition: transform 220ms ease, border-color 220ms ease;
}

.resource-card::before {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  opacity: 0;
  background: radial-gradient(circle at top left, rgb(124 92 255 / 0.18), transparent 45%);
  transition: opacity 220ms ease;
}

.resource-card:hover,
.resource-card:focus-within {
  transform: translateY(-4px);
  border-color: rgb(255 255 255 / 0.28);
}

.resource-card:hover::before,
.resource-card:focus-within::before {
  opacity: 1;
}

@container (min-width: 28rem) {
  .resource-card__content {
    padding: 1.75rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .resource-card,
  .resource-card::before {
    transition: none;
  }
}
~~~

</details>

## Stretch goals

- Sort featured items first.
- Add a `publishedAt` field and sort newest first.
- Make the whole card clickable without invalid nested interactive elements.
- Add a second layout variant using only container queries.