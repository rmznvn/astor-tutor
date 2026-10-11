# Task 03 — Explanation

## Why React belongs here

The resource data itself does not need React.

Astro can load it once.

The filter controls do need browser state:

- current query
- selected topic
- selected difficulty
- selected content type

So React becomes a small interactive island.

~~~text
Astro
  ↓ loads content
resources[]
  ↓ props
React island
  ↓
filter state + UI
~~~

## Why not fetch again in React?

Astro already has the collection data.

Fetching the same data again would:

- duplicate work
- introduce loading states unnecessarily
- complicate data ownership

Passing serializable data as props is cleaner.

## Derived state

Do not do this unless necessary:

~~~js
const [filteredResources, setFilteredResources] = useState([]);
~~~

The filtered list is derived from:

- resources
- query
- topic
- difficulty

So calculate it from those values, often with `useMemo()`.

## Why URL state matters

Without URL state:

~~~text
filter → refresh → state disappears
~~~

With URL state:

~~~text
/resources?topic=astro&difficulty=intermediate
~~~

the view becomes:

- shareable
- restorable
- compatible with browser navigation

## Why `popstate`?

`replaceState()` changes the URL without reloading.

But when the user presses Back or Forward, React must read the URL again and update its state.

That is what the `popstate` event is for.

## Interview questions

### Why not make the entire page React?

A strong answer:

Only filtering requires client-side state. Astro can render the rest without shipping unnecessary JavaScript.

### Why use derived state?

A strong answer:

If a value can be computed from existing props/state, storing another copy risks synchronization bugs.

### Why store filters in the URL?

A strong answer:

It makes the state shareable, refresh-safe and compatible with browser history.
