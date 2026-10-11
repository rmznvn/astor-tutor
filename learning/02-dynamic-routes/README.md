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
- rendering collection content
- layouts and reusable page structure
- small client-side scripts inside an otherwise static Astro page

## Requirements

Create a resources listing page and a dynamic detail page.

The detail page should include:

- back link
- topic/difficulty metadata
- title and description
- rendered article body
- sticky table of contents
- reading progress indicator
- Previous / Next navigation

## CSS practice

Use:

- CSS custom properties
- `position: sticky`
- `scroll-margin-top`
- `text-wrap: balance`
- `:has()` where it improves UI state
- gradient/pseudo-element decoration

## Interaction

Use a small browser script for:

- reading progress
- active heading in the TOC

Do not convert the whole article to React.

## Acceptance criteria

- Each collection item gets a valid detail URL.
- URLs are generated from the dynamic route.
- Article content is rendered from the collection entry.
- TOC links jump to headings.
- reading progress reaches 100% near the article end.
- reduced motion and keyboard users are not blocked.
- `npm run build` succeeds.

## Hints

1. `getStaticPaths()` answers: which pages must exist at build time?
2. Return both `params` and `props` so the page does not need to query the same entry again.
3. Keep the progress script tiny; this page does not need a framework island.
4. Build Previous/Next from the sorted collection array.

## Explanation

With SSG, Astro must know every dynamic URL at build time. `getStaticPaths()` creates that list. The route file is a template; each returned params object creates one page.

A tiny inline browser script is appropriate for scroll state because the article itself is static HTML. React would add unnecessary hydration cost for this feature.

### Interview question

**Why does a static dynamic route need `getStaticPaths()`?**

A strong answer: because during a static build Astro must enumerate every concrete URL that should be generated from the dynamic route template.

### Interview question

**When would SSR remove the need to pre-generate every slug?**

A strong answer: when the page is rendered on request by a server adapter, the current URL parameters can be handled at request time instead of being enumerated during build.

<details>
<summary><strong>Reference solution — open after your attempt</strong></summary>

### Route skeleton

~~~astro
---
import { getCollection, render } from 'astro:content';

export async function getStaticPaths() {
  const resources = await getCollection('resources');

  return resources.map((resource, index) => ({
    params: { slug: resource.id },
    props: {
      resource,
      previous: resources[index - 1] ?? null,
      next: resources[index + 1] ?? null,
    },
  }));
}

const { resource, previous, next } = Astro.props;
const { Content, headings } = await render(resource);
---

<div class="reading-progress" data-progress></div>

<main class="article-layout">
  <aside class="toc">
    <p>On this page</p>
    <nav>
      {headings.filter((heading) => heading.depth === 2).map((heading) => (
        <a href={`#${heading.slug}`} data-toc-link={heading.slug}>{heading.text}</a>
      ))}
    </nav>
  </aside>

  <article class="article" data-article>
    <h1>{resource.data.title}</h1>
    <Content />
  </article>
</main>

<nav class="article-nav">
  {previous && <a href={`/resources/${previous.id}`}>← {previous.data.title}</a>}
  {next && <a href={`/resources/${next.id}`}>{next.data.title} →</a>}
</nav>

<script>
  const progress = document.querySelector('[data-progress]');
  const article = document.querySelector('[data-article]');

  function updateProgress() {
    if (!progress || !article) return;
    const rect = article.getBoundingClientRect();
    const total = article.offsetHeight - window.innerHeight;
    const travelled = Math.min(Math.max(-rect.top, 0), Math.max(total, 1));
    progress.style.setProperty('--progress', `${travelled / Math.max(total, 1) * 100}%`);
  }

  updateProgress();
  window.addEventListener('scroll', updateProgress, { passive: true });
</script>
~~~

### CSS direction

~~~css
.reading-progress {
  --progress: 0%;
  position: fixed;
  inset: 0 0 auto;
  z-index: 20;
  height: 3px;
  background: linear-gradient(90deg, currentColor var(--progress), transparent 0);
}

.article-layout {
  display: grid;
  grid-template-columns: minmax(10rem, 14rem) minmax(0, 46rem);
  gap: clamp(2rem, 6vw, 5rem);
}

.toc {
  position: sticky;
  top: 2rem;
  align-self: start;
}

.article :is(h2, h3) {
  scroll-margin-top: 5rem;
}

.article h1 {
  text-wrap: balance;
}
~~~

</details>

## Stretch goals

- Highlight the active TOC link with `IntersectionObserver`.
- Add estimated reading time automatically.
- Add breadcrumb JSON-LD.
- Add a copy-link button for headings.