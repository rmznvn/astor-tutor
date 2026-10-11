# Task 01 — Reference Solution

Compare this with your own implementation only after you have tried the task.

## 1. `src/content.config.ts`

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

## 2. Example content files

### `src/content/resources/astro-routing.md`

~~~md
---
title: "Understanding Astro Routing"
description: "Learn how Astro maps files to URLs and when dynamic routes are useful."
topic: "Astro"
readingTime: 7
featured: true
---
~~~

### `src/content/resources/css-container-queries.md`

~~~md
---
title: "CSS Container Queries"
description: "Build components that respond to the space available to them."
topic: "CSS"
readingTime: 6
featured: false
---
~~~

### `src/content/resources/array-methods.md`

~~~md
---
title: "JavaScript Array Methods"
description: "Practice map, filter and find with practical frontend examples."
topic: "JavaScript"
readingTime: 8
featured: false
---
~~~

## 3. `src/components/ResourceCard.astro`

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

    <p class="resource-card__description">
      {description}
    </p>

    <div class="resource-card__meta">
      <span>{readingTime} min read</span>
      <span class="resource-card__arrow" aria-hidden="true">→</span>
    </div>
  </div>
</article>
~~~

## 4. `src/components/ResourcesGrid.astro`

~~~astro
---
import ResourceCard from './ResourceCard.astro';

const { resources } = Astro.props;
---

<section class="resources" aria-labelledby="resources-title">
  <div class="resources__heading">
    <p>Learn</p>
    <h1 id="resources-title">Latest Resources</h1>
  </div>

  <div class="resources__grid">
    {resources.map((resource) => (
      <ResourceCard resource={resource} />
    ))}
  </div>
</section>
~~~

## 5. `src/pages/index.astro`

~~~astro
---
import { getCollection } from 'astro:content';
import ResourcesGrid from '../components/ResourcesGrid.astro';
import '../styles/global.css';

const resources = await getCollection('resources');

const sortedResources = [...resources].sort((a, b) => {
  return Number(b.data.featured) - Number(a.data.featured);
});
---

<ResourcesGrid resources={sortedResources} />
~~~

## 6. CSS

~~~css
.resources {
  width: min(72rem, calc(100% - 2rem));
  margin-inline: auto;
  padding-block: clamp(3rem, 8vw, 7rem);
}

.resources__heading {
  max-width: 42rem;
  margin-bottom: clamp(1.5rem, 4vw, 3rem);
}

.resources__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 18rem), 1fr));
  gap: clamp(1rem, 2vw, 1.5rem);
}

.resource-card {
  container-type: inline-size;
  position: relative;
  isolation: isolate;
  overflow: hidden;
  min-height: 22rem;
  border: 1px solid rgb(255 255 255 / 0.12);
  border-radius: 1.25rem;
  background: rgb(255 255 255 / 0.035);
  transition:
    transform 220ms ease,
    border-color 220ms ease;
}

.resource-card::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  opacity: 0;
  background:
    radial-gradient(circle at 15% 10%, rgb(124 92 255 / 0.22), transparent 38%),
    linear-gradient(145deg, transparent, rgb(255 255 255 / 0.03));
  transition: opacity 220ms ease;
}

.resource-card__media {
  aspect-ratio: 16 / 8;
  background:
    linear-gradient(135deg, rgb(124 92 255 / 0.25), transparent),
    rgb(255 255 255 / 0.04);
  transition: transform 300ms ease;
}

.resource-card__content {
  padding: clamp(1rem, 4cqw, 1.5rem);
}

.resource-card__topic {
  margin: 0 0 0.75rem;
  font-size: 0.75rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  opacity: 0.72;
}

.resource-card h2 {
  margin: 0;
  font-size: clamp(1.25rem, 8cqw, 1.75rem);
  text-wrap: balance;
}

.resource-card__description {
  line-height: 1.6;
  opacity: 0.8;
}

.resource-card__meta {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  margin-top: 1.5rem;
}

.resource-card__arrow {
  transition: transform 220ms ease;
}

.resource-card:is(:hover, :focus-within) {
  transform: translateY(-4px);
  border-color: rgb(255 255 255 / 0.3);
}

.resource-card:is(:hover, :focus-within)::before {
  opacity: 1;
}

.resource-card:is(:hover, :focus-within) .resource-card__media {
  transform: scale(1.025);
}

.resource-card:is(:hover, :focus-within) .resource-card__arrow {
  transform: translateX(4px);
}

.resource-card.is-featured {
  grid-column: span 2;
}

@container (max-width: 24rem) {
  .resource-card__description {
    font-size: 0.9375rem;
  }
}

@media (max-width: 42rem) {
  .resource-card.is-featured {
    grid-column: auto;
  }
}

@media (prefers-reduced-motion: reduce) {
  .resource-card,
  .resource-card::before,
  .resource-card__media,
  .resource-card__arrow {
    transition: none;
  }
}
~~~

## Alternative approach

For very small static data you could import a JSON file directly. The Content Collection approach is stronger here because later tasks will reuse the same resources for dynamic routes and filtering.
