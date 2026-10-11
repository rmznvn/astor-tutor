# Task 03 — Filterable Resource Library with a React Island

## Goal

Build a filterable resources page where Astro loads the content and React manages only the interactive filtering UI.

Example state:

~~~text
/resources?topic=astro&difficulty=intermediate&q=routing
~~~

## You will practice

- Astro + React islands
- `client:load`
- passing server-loaded data into a React component
- `useState`
- derived filtered data
- controlled inputs
- `URLSearchParams`
- browser history
- progressive enhancement decisions

## Requirements

Filters:

- search query
- topic
- difficulty
- content type
- clear all

The URL must reflect active filters so refresh/back/forward preserve useful state.

Show the result count and a useful empty state.

On mobile, filters should open in a drawer.

## CSS/UI practice

Practice:

- custom filter chips
- `focus-visible`
- `backdrop-filter`
- sticky filter bar
- motion using opacity/transform
- `prefers-reduced-motion`

You may optionally use shadcn/ui for the mobile Sheet and accessible controls.

## Acceptance criteria

- Astro queries the resources.
- React does not fetch the same content again.
- React owns filter state only.
- active filters are encoded in the URL.
- refresh restores filters from the URL.
- back/forward updates the UI correctly.
- empty state is clear and actionable.
- keyboard focus states are visible.

## Hints

1. Start without URL state. Make filtering work first.
2. Then initialize state from `new URLSearchParams(window.location.search)`.
3. Use `history.replaceState()` while the user changes filters.
4. Derived results usually should not be stored in separate state; calculate them from the current filters and resources.

## Explanation

This is a strong Astro use case for a React island: the content can remain server/static-rendered in concept, while the filter controls need browser state and immediate interaction.

Passing already loaded data into the island prevents duplicate network work and keeps data ownership clear.

### Interview question

**Why not make the entire page React?**

A strong answer: only the filtering interaction needs client state. Astro can render the surrounding page and data efficiently without shipping JavaScript for static UI.

### Interview question

**Why keep filter state in the URL?**

A strong answer: it makes filtered views shareable, restorable after refresh, and compatible with browser navigation.

<details>
<summary><strong>Reference solution — open after your attempt</strong></summary>

### Add React

~~~bash
npx astro add react
~~~

### Page

~~~astro
---
import { getCollection } from 'astro:content';
import ResourceFilter from '../../components/ResourceFilter';

const resources = await getCollection('resources');
const serializableResources = resources.map((resource) => ({
  id: resource.id,
  ...resource.data,
}));
---

<ResourceFilter resources={serializableResources} client:load />
~~~

### React island

~~~tsx
import { useEffect, useMemo, useState } from 'react';

type Resource = {
  id: string;
  title: string;
  description: string;
  topic: string;
  difficulty?: string;
  contentType?: string;
};

export default function ResourceFilter({ resources }: { resources: Resource[] }) {
  const [query, setQuery] = useState('');
  const [topic, setTopic] = useState('all');
  const [difficulty, setDifficulty] = useState('all');

  useEffect(() => {
    const params = new URLSearchParams(window.location.search);
    setQuery(params.get('q') ?? '');
    setTopic(params.get('topic') ?? 'all');
    setDifficulty(params.get('difficulty') ?? 'all');
  }, []);

  useEffect(() => {
    const params = new URLSearchParams();
    if (query) params.set('q', query);
    if (topic !== 'all') params.set('topic', topic);
    if (difficulty !== 'all') params.set('difficulty', difficulty);

    const nextUrl = params.size ? `?${params.toString()}` : window.location.pathname;
    window.history.replaceState(null, '', nextUrl);
  }, [query, topic, difficulty]);

  const filtered = useMemo(() => {
    const normalizedQuery = query.trim().toLowerCase();

    return resources.filter((resource) => {
      const matchesQuery = !normalizedQuery ||
        `${resource.title} ${resource.description}`.toLowerCase().includes(normalizedQuery);
      const matchesTopic = topic === 'all' || resource.topic === topic;
      const matchesDifficulty = difficulty === 'all' || resource.difficulty === difficulty;

      return matchesQuery && matchesTopic && matchesDifficulty;
    });
  }, [resources, query, topic, difficulty]);

  return (
    <section className="library">
      <div className="filters">
        <input
          type="search"
          value={query}
          onChange={(event) => setQuery(event.target.value)}
          placeholder="Search resources"
        />

        <button type="button" onClick={() => setTopic('astro')}>Astro</button>
        <button type="button" onClick={() => setTopic('all')}>All topics</button>
      </div>

      <p aria-live="polite">{filtered.length} resources</p>

      {filtered.length ? (
        <div className="resource-grid">
          {filtered.map((resource) => (
            <article key={resource.id} className="filter-card">
              <p>{resource.topic}</p>
              <h2>{resource.title}</h2>
              <p>{resource.description}</p>
            </article>
          ))}
        </div>
      ) : (
        <div className="empty-state">
          <h2>No resources found</h2>
          <button type="button" onClick={() => {
            setQuery('');
            setTopic('all');
            setDifficulty('all');
          }}>Clear filters</button>
        </div>
      )}
    </section>
  );
}
~~~

### Important next improvement

The reference code above initializes from the URL but does not yet listen to the browser `popstate` event. Add that yourself so Back/Forward updates the controls.

</details>

## Stretch goals

- Add `popstate` handling.
- Add debounced search without breaking immediate chip filters.
- Add animated filter chips.
- Add a shadcn/ui mobile Sheet.
- Move reusable URL-filter logic into a custom React hook.
- Add tests for filter logic.