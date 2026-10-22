# Task 04 — Reference Solution

## 1. Add a schema

Update `src/content.config.ts`:

~~~ts
import { defineCollection } from 'astro:content';
import { glob } from 'astro/loaders';
import { z } from 'astro/zod';

const resources = defineCollection({
  loader: glob({
    base: './src/content/resources',
    pattern: '**/*.md',
  }),
  schema: ({ image }) =>
    z.object({
      title: z.string(),
      description: z.string(),
      topic: z.string(),
      readingTime: z.number(),
      featured: z.boolean(),
      cover: image(),
      coverAlt: z.string().min(1),
    }),
});

export const collections = { resources };
~~~

## 2. Reference a local image

Example frontmatter:

~~~md
---
title: "Understanding Astro Routing"
description: "Learn how Astro maps files to URLs."
topic: "Astro"
readingTime: 7
featured: true
cover: "../../assets/resource-covers/astro-routing.jpg"
coverAlt: "Code editor showing an Astro dynamic route file"
---
~~~

## 3. Render through astro:assets

In `ResourceCard.astro`:

~~~astro
---
import { Image } from 'astro:assets';

const { resource, priority = false } = Astro.props;
const {
  title,
  description,
  topic,
  readingTime,
  cover,
  coverAlt,
} = resource.data;
---

<article class="resource-card">
  <div class="resource-card__media">
    <Image
      src={cover}
      alt={coverAlt}
      layout="full-width"
      priority={priority}
      class="resource-card__image"
    />
    <div class="resource-card__overlay" aria-hidden="true"></div>
  </div>

  <div class="resource-card__body">
    <p>{topic}</p>
    <h2>{title}</h2>
    <p>{description}</p>
    <span>{readingTime} min read</span>
  </div>
</article>
~~~

Pass `priority={true}` only to the one featured card that appears above the fold.

## 4. CSS

~~~css
.resource-card {
  container-type: inline-size;
  overflow: clip;
  border-radius: 1.25rem;
}

.resource-card__media {
  position: relative;
  aspect-ratio: 16 / 9;
  overflow: clip;
}

.resource-card__image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  transition: transform 420ms cubic-bezier(.2,.8,.2,1);
}

.resource-card__overlay {
  position: absolute;
  inset: 0;
  pointer-events: none;
  background:
    linear-gradient(to top, rgb(0 0 0 / .42), transparent 55%),
    radial-gradient(circle at 20% 15%, rgb(130 100 255 / .22), transparent 40%);
  opacity: .8;
  transition: opacity 260ms ease;
}

.resource-card__body h2 {
  transition: transform 260ms ease;
}

.resource-card:is(:hover, :focus-within) .resource-card__image {
  transform: scale(1.035);
}

.resource-card:is(:hover, :focus-within) .resource-card__overlay {
  opacity: 1;
}

.resource-card:is(:hover, :focus-within) .resource-card__body h2 {
  transform: translateX(.2rem);
}

@container (max-width: 22rem) {
  .resource-card__media {
    aspect-ratio: 4 / 3;
  }
}

@media (prefers-reduced-motion: reduce) {
  .resource-card__image,
  .resource-card__overlay,
  .resource-card__body h2 {
    transition: none;
  }
}
~~~

## What to inspect

Open DevTools and inspect the generated image markup. Compare it with a plain image served from `public/`.

## Alternative

For remote images, configure approved remote domains and use remote image metadata appropriately. Do not move every remote image into `public/` just to avoid configuration.
