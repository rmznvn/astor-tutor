# Task 03 — Reference Solution

## 1. Add React

~~~bash
npx astro add react
~~~

## 2. Update content fields

For this task your resources should also contain:

~~~md
---
title: "Understanding Astro Routing"
description: "Learn how Astro maps files to URLs."
topic: "Astro"
difficulty: "intermediate"
contentType: "article"
readingTime: 7
featured: true
---
~~~

## 3. `src/pages/resources/index.astro`

~~~astro
---
import { getCollection } from 'astro:content';
import ResourceFilter from '../../components/ResourceFilter';

const resources = await getCollection('resources');

const serializableResources = resources.map((resource) => ({
  id: resource.id,
  title: resource.data.title,
  description: resource.data.description,
  topic: resource.data.topic,
  difficulty: resource.data.difficulty,
  contentType: resource.data.contentType,
  readingTime: resource.data.readingTime,
}));
---

<main>
  <h1>Resources</h1>

  <ResourceFilter
    resources={serializableResources}
    client:load
  />
</main>
~~~

## 4. `src/components/ResourceFilter.tsx`

~~~tsx
import {
  useEffect,
  useMemo,
  useState,
} from 'react';

type Resource = {
  id: string;
  title: string;
  description: string;
  topic: string;
  difficulty: string;
  contentType: string;
  readingTime: number;
};

type Filters = {
  query: string;
  topic: string;
  difficulty: string;
  contentType: string;
};

const DEFAULT_FILTERS: Filters = {
  query: '',
  topic: 'all',
  difficulty: 'all',
  contentType: 'all',
};

function readFiltersFromUrl(): Filters {
  const params = new URLSearchParams(window.location.search);

  return {
    query: params.get('q') ?? '',
    topic: params.get('topic') ?? 'all',
    difficulty: params.get('difficulty') ?? 'all',
    contentType: params.get('type') ?? 'all',
  };
}

function writeFiltersToUrl(filters: Filters) {
  const params = new URLSearchParams();

  if (filters.query) params.set('q', filters.query);
  if (filters.topic !== 'all') params.set('topic', filters.topic);
  if (filters.difficulty !== 'all') {
    params.set('difficulty', filters.difficulty);
  }
  if (filters.contentType !== 'all') {
    params.set('type', filters.contentType);
  }

  const queryString = params.toString();

  const nextUrl = queryString
    ? `${window.location.pathname}?${queryString}`
    : window.location.pathname;

  window.history.replaceState(null, '', nextUrl);
}

export default function ResourceFilter({
  resources,
}: {
  resources: Resource[];
}) {
  const [filters, setFilters] = useState<Filters>(DEFAULT_FILTERS);
  const [ready, setReady] = useState(false);

  useEffect(() => {
    setFilters(readFiltersFromUrl());
    setReady(true);

    function handlePopState() {
      setFilters(readFiltersFromUrl());
    }

    window.addEventListener('popstate', handlePopState);

    return () => {
      window.removeEventListener('popstate', handlePopState);
    };
  }, []);

  useEffect(() => {
    if (!ready) return;
    writeFiltersToUrl(filters);
  }, [filters, ready]);

  const topics = useMemo(
    () => [...new Set(resources.map((resource) => resource.topic))],
    [resources]
  );

  const filteredResources = useMemo(() => {
    const normalizedQuery = filters.query.trim().toLowerCase();

    return resources.filter((resource) => {
      const searchableText =
        `${resource.title} ${resource.description} ${resource.topic}`
          .toLowerCase();

      const matchesQuery =
        !normalizedQuery ||
        searchableText.includes(normalizedQuery);

      const matchesTopic =
        filters.topic === 'all' ||
        resource.topic === filters.topic;

      const matchesDifficulty =
        filters.difficulty === 'all' ||
        resource.difficulty === filters.difficulty;

      const matchesType =
        filters.contentType === 'all' ||
        resource.contentType === filters.contentType;

      return (
        matchesQuery &&
        matchesTopic &&
        matchesDifficulty &&
        matchesType
      );
    });
  }, [resources, filters]);

  function clearFilters() {
    setFilters(DEFAULT_FILTERS);
  }

  const hasActiveFilters =
    filters.query !== '' ||
    filters.topic !== 'all' ||
    filters.difficulty !== 'all' ||
    filters.contentType !== 'all';

  return (
    <section className="library">
      <div className="filter-bar">
        <label className="search-field">
          <span className="sr-only">Search resources</span>

          <input
            type="search"
            value={filters.query}
            placeholder="Search resources"
            onChange={(event) =>
              setFilters((current) => ({
                ...current,
                query: event.target.value,
              }))
            }
          />
        </label>

        <div className="filter-group" aria-label="Topics">
          <button
            type="button"
            aria-pressed={filters.topic === 'all'}
            onClick={() =>
              setFilters((current) => ({
                ...current,
                topic: 'all',
              }))
            }
          >
            All
          </button>

          {topics.map((topic) => (
            <button
              key={topic}
              type="button"
              aria-pressed={filters.topic === topic}
              onClick={() =>
                setFilters((current) => ({
                  ...current,
                  topic,
                }))
              }
            >
              {topic}
            </button>
          ))}
        </div>

        <label>
          <span>Difficulty</span>

          <select
            value={filters.difficulty}
            onChange={(event) =>
              setFilters((current) => ({
                ...current,
                difficulty: event.target.value,
              }))
            }
          >
            <option value="all">All</option>
            <option value="beginner">Beginner</option>
            <option value="intermediate">Intermediate</option>
            <option value="advanced">Advanced</option>
          </select>
        </label>

        <label>
          <span>Type</span>

          <select
            value={filters.contentType}
            onChange={(event) =>
              setFilters((current) => ({
                ...current,
                contentType: event.target.value,
              }))
            }
          >
            <option value="all">All</option>
            <option value="article">Article</option>
            <option value="task">Task</option>
            <option value="guide">Guide</option>
          </select>
        </label>

        {hasActiveFilters && (
          <button type="button" onClick={clearFilters}>
            Clear all
          </button>
        )}
      </div>

      <p className="result-count" aria-live="polite">
        {filteredResources.length} resources
      </p>

      {filteredResources.length > 0 ? (
        <div className="resource-grid">
          {filteredResources.map((resource) => (
            <article className="filter-card" key={resource.id}>
              <p>{resource.topic}</p>
              <h2>
                <a href={`/resources/${resource.id}`}>
                  {resource.title}
                </a>
              </h2>
              <p>{resource.description}</p>
              <small>
                {resource.difficulty} · {resource.readingTime} min
              </small>
            </article>
          ))}
        </div>
      ) : (
        <div className="empty-state">
          <p>No resources match these filters.</p>
          <button type="button" onClick={clearFilters}>
            Clear filters
          </button>
        </div>
      )}
    </section>
  );
}
~~~

## 5. CSS direction

~~~css
.filter-bar {
  position: sticky;
  top: 1rem;
  z-index: 10;
  display: grid;
  gap: 1rem;
  padding: 1rem;
  border: 1px solid rgb(255 255 255 / 0.12);
  border-radius: 1rem;
  background: rgb(15 16 20 / 0.72);
  backdrop-filter: blur(18px);
}

.filter-group {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.filter-group button {
  border: 1px solid rgb(255 255 255 / 0.14);
  border-radius: 999px;
  padding: 0.55rem 0.85rem;
  background: transparent;
  color: inherit;
  transition:
    transform 180ms ease,
    background-color 180ms ease;
}

.filter-group button[aria-pressed="true"] {
  background: rgb(255 255 255 / 0.12);
}

.filter-group button:hover {
  transform: translateY(-2px);
}

:is(button, input, select):focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 3px;
}

.resource-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 18rem), 1fr));
  gap: 1rem;
  margin-top: 1.5rem;
}

.filter-card {
  border: 1px solid rgb(255 255 255 / 0.1);
  border-radius: 1rem;
  padding: 1.25rem;
  animation: card-enter 240ms ease both;
}

@keyframes card-enter {
  from {
    opacity: 0;
    transform: translateY(8px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.empty-state {
  margin-top: 2rem;
  padding: 2rem;
  border: 1px dashed rgb(255 255 255 / 0.2);
  border-radius: 1rem;
  text-align: center;
}

@media (prefers-reduced-motion: reduce) {
  .filter-group button,
  .filter-card {
    transition: none;
    animation: none;
  }
}
~~~

## Note about the mobile drawer

For your first implementation, build the filtering logic before introducing a component library.

After it works, replace the mobile filter panel with a shadcn/ui `Sheet`. This keeps the learning order clear:

1. state logic
2. URL synchronization
3. responsive UI
4. library integration
