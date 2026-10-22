# Task 06 — Reference Solution

Create:

~~~text
src/pages/resources/archive/[...page].astro
~~~

## Route

~~~astro
---
import type { GetStaticPaths } from 'astro';
import { getCollection } from 'astro:content';

export const getStaticPaths = (async ({ paginate }) => {
  const resources = await getCollection('resources');

  const sorted = [...resources].sort((a, b) =>
    a.data.title.localeCompare(b.data.title)
  );

  return paginate(sorted, {
    pageSize: 6,
  });
}) satisfies GetStaticPaths;

const { page } = Astro.props;

const pages = Array.from(
  { length: page.lastPage },
  (_, index) => index + 1
);

function pageHref(pageNumber: number) {
  return pageNumber === 1
    ? '/resources/archive'
    : `/resources/archive/${pageNumber}`;
}
---

<main class="archive">
  <header class="archive__header">
    <p>Resource archive</p>
    <h1>Browse everything</h1>
    <p>Page {page.currentPage} of {page.lastPage}</p>
  </header>

  <div class="archive__grid">
    {page.data.map((resource) => (
      <article class="archive-card">
        <p>{resource.data.topic}</p>
        <h2>
          <a href={`/resources/${resource.id}`}>
            {resource.data.title}
          </a>
        </h2>
      </article>
    ))}
  </div>

  <nav class="pagination" aria-label="Resource archive pages">
    {page.url.prev ? (
      <a href={page.url.prev}>← Previous</a>
    ) : (
      <span aria-disabled="true">← Previous</span>
    )}

    <div class="pagination__numbers">
      {pages.map((number) => (
        <a
          href={pageHref(number)}
          aria-current={number === page.currentPage ? 'page' : undefined}
        >
          {number}
        </a>
      ))}
    </div>

    {page.url.next ? (
      <a href={page.url.next}>Next →</a>
    ) : (
      <span aria-disabled="true">Next →</span>
    )}
  </nav>
</main>
~~~

## CSS

~~~css
.archive {
  inline-size: min(72rem, calc(100% - 2rem));
  margin-inline: auto;
  padding-block: clamp(3rem, 8vw, 7rem);
}

.archive__grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 17rem), 1fr));
  gap: 1rem;
}

.archive-card {
  min-inline-size: 0;
  padding: 1.25rem;
  border: 1px solid rgb(255 255 255 / .12);
  border-radius: 1rem;
  opacity: 1;
  transform: translateY(0);
  transition:
    opacity 280ms ease,
    transform 280ms ease;
}

@starting-style {
  .archive-card {
    opacity: 0;
    transform: translateY(.75rem);
  }
}

.pagination {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  gap: 1rem;
  align-items: center;
  margin-block-start: 3rem;
}

.pagination > :last-child {
  justify-self: end;
}

.pagination__numbers {
  display: flex;
  flex-wrap: wrap;
  gap: .4rem;
}

.pagination a,
.pagination span {
  min-inline-size: 2.5rem;
  min-block-size: 2.5rem;
  display: inline-grid;
  place-items: center;
  border-radius: .75rem;
}

.pagination a[aria-current="page"] {
  background: rgb(255 255 255 / .12);
}

.pagination:has(a:focus-visible) {
  outline: 1px solid rgb(255 255 255 / .18);
  outline-offset: .4rem;
}

.pagination a:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 2px;
}

@media (prefers-reduced-motion: reduce) {
  .archive-card {
    transition: none;
  }
}
~~~

## Important product note

Your React filters from Task 03 and this static archive solve different problems. The archive demonstrates server/build-time pagination. Do not silently combine client filtering with only one page of a larger paginated dataset unless that behavior is intentional.
