# Task 02 — Reference Solution

## 1. Give your Markdown body content

Example:

~~~md
---
title: "Understanding Astro Routing"
description: "Learn how Astro maps files to URLs."
topic: "Astro"
readingTime: 7
featured: true
---

## Static routes

Astro uses the file system to create routes.

## Dynamic routes

Dynamic segments use square brackets.

## getStaticPaths

Static builds need all dynamic URLs in advance.
~~~

## 2. `src/pages/resources/index.astro`

~~~astro
---
import { getCollection } from 'astro:content';

const resources = await getCollection('resources');
---

<main>
  <h1>Resources</h1>

  <ul>
    {resources.map((resource) => (
      <li>
        <a href={`/resources/${resource.id}`}>
          {resource.data.title}
        </a>
      </li>
    ))}
  </ul>
</main>
~~~

## 3. `src/pages/resources/[slug].astro`

~~~astro
---
import { getCollection, render } from 'astro:content';

export async function getStaticPaths() {
  const resources = await getCollection('resources');

  const sorted = [...resources].sort((a, b) =>
    a.data.title.localeCompare(b.data.title)
  );

  return sorted.map((resource, index) => ({
    params: { slug: resource.id },
    props: {
      resource,
      previous: sorted[index - 1] ?? null,
      next: sorted[index + 1] ?? null,
    },
  }));
}

const { resource, previous, next } = Astro.props;
const { Content, headings } = await render(resource);

const tocHeadings = headings.filter((heading) => heading.depth === 2);
---

<div class="reading-progress" data-progress aria-hidden="true"></div>

<header class="article-hero">
  <a href="/resources">← Back to Resources</a>
  <p>{resource.data.topic} · {resource.data.readingTime} min read</p>
  <h1>{resource.data.title}</h1>
  <p>{resource.data.description}</p>
</header>

<div class="article-layout">
  <aside class="toc">
    <p class="toc__label">On this page</p>

    <nav aria-label="Table of contents">
      {tocHeadings.map((heading) => (
        <a
          href={`#${heading.slug}`}
          data-toc-link={heading.slug}
        >
          {heading.text}
        </a>
      ))}
    </nav>
  </aside>

  <article class="article" data-article>
    <Content />
  </article>
</div>

<nav class="article-nav" aria-label="Article navigation">
  <div>
    {previous && (
      <a href={`/resources/${previous.id}`}>
        ← {previous.data.title}
      </a>
    )}
  </div>

  <div>
    {next && (
      <a href={`/resources/${next.id}`}>
        {next.data.title} →
      </a>
    )}
  </div>
</nav>

<script>
  const progress = document.querySelector('[data-progress]');
  const article = document.querySelector('[data-article]');

  function updateProgress() {
    if (!(progress instanceof HTMLElement)) return;
    if (!(article instanceof HTMLElement)) return;

    const rect = article.getBoundingClientRect();
    const articleStart = window.scrollY + rect.top;
    const maxScrollable = article.offsetHeight - window.innerHeight;
    const travelled = window.scrollY - articleStart;

    const ratio = maxScrollable > 0
      ? Math.min(Math.max(travelled / maxScrollable, 0), 1)
      : 1;

    progress.style.setProperty('--progress', `${ratio * 100}%`);
  }

  updateProgress();
  window.addEventListener('scroll', updateProgress, { passive: true });

  const links = Array.from(
    document.querySelectorAll('[data-toc-link]')
  );

  const headings = links
    .map((link) => {
      if (!(link instanceof HTMLAnchorElement)) return null;

      const id = link.dataset.tocLink;
      const heading = id ? document.getElementById(id) : null;

      return heading ? { link, heading } : null;
    })
    .filter(Boolean);

  const observer = new IntersectionObserver(
    (entries) => {
      const visible = entries.find((entry) => entry.isIntersecting);
      if (!visible) return;

      links.forEach((link) => link.removeAttribute('aria-current'));

      const active = links.find(
        (link) => link.dataset.tocLink === visible.target.id
      );

      active?.setAttribute('aria-current', 'true');
    },
    {
      rootMargin: '-20% 0px -65% 0px',
      threshold: 0,
    }
  );

  headings.forEach((item) => {
    if (item) observer.observe(item.heading);
  });
</script>
~~~

## 4. CSS

~~~css
.reading-progress {
  --progress: 0%;
  position: fixed;
  inset: 0 0 auto;
  z-index: 100;
  height: 3px;
  pointer-events: none;
  background:
    linear-gradient(
      90deg,
      currentColor 0 var(--progress),
      transparent var(--progress) 100%
    );
}

.article-hero {
  width: min(56rem, calc(100% - 2rem));
  margin-inline: auto;
  padding-block: clamp(3rem, 8vw, 7rem);
}

.article-hero h1 {
  font-size: clamp(2.5rem, 8vw, 5.5rem);
  line-height: 0.98;
  text-wrap: balance;
}

.article-layout {
  width: min(76rem, calc(100% - 2rem));
  margin-inline: auto;
  display: grid;
  grid-template-columns: minmax(11rem, 15rem) minmax(0, 46rem);
  gap: clamp(2rem, 6vw, 6rem);
  align-items: start;
}

.toc {
  position: sticky;
  top: 2rem;
}

.toc nav {
  display: grid;
  gap: 0.5rem;
}

.toc a {
  opacity: 0.55;
  transition:
    opacity 180ms ease,
    transform 180ms ease;
}

.toc a[aria-current="true"] {
  opacity: 1;
  transform: translateX(0.35rem);
}

.article {
  min-width: 0;
}

.article :is(h2, h3) {
  scroll-margin-top: 5rem;
}

.article :is(h2, h3):has(+ p) {
  margin-bottom: 0.65rem;
}

.article-nav {
  width: min(76rem, calc(100% - 2rem));
  margin: 4rem auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.article-nav > :last-child {
  text-align: right;
}

@media (max-width: 48rem) {
  .article-layout {
    grid-template-columns: 1fr;
  }

  .toc {
    position: static;
  }
}

@media (prefers-reduced-motion: reduce) {
  .toc a {
    transition: none;
  }
}
~~~

## Alternative

If this project later uses SSR, the route can read `Astro.params.slug` at request time instead of pre-generating every page with `getStaticPaths()`.
