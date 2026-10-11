# Task 03 — Filterable Resource Library with a React Island

## Goal

Build a filterable resources page where Astro loads content and React manages only the interactive filtering state.

Example:

~~~text
/resources?topic=astro&difficulty=intermediate&q=routing
~~~

## You will practice

- Astro + React islands
- `client:load`
- props from Astro to React
- `useState`
- `useMemo`
- controlled inputs
- URL state
- `URLSearchParams`
- `history.replaceState()`
- `popstate`
- responsive filter UI

## Requirements

Filters:

- search query
- topic
- difficulty
- content type
- clear all

Also include:

- result count
- active filter chips
- empty state
- mobile filters drawer
- URL synchronization

## CSS/UI practice

Use:

- `:focus-visible`
- sticky toolbar
- `backdrop-filter`
- filter chips
- CSS transitions
- opacity/transform animations
- `prefers-reduced-motion`

Optional:

- shadcn/ui Sheet for the mobile drawer

## Acceptance criteria

- Astro loads the resources.
- React does not fetch the same resources again.
- React manages filter state only.
- URL updates when filters change.
- Refresh restores filter state.
- Back/Forward restores filter state.
- Empty state includes a Clear Filters action.
- Result count is announced accessibly.
- Mobile filter UI is usable.
- `npm run build` succeeds.

## Hints

1. First make filtering work with local state.
2. Derived results usually do not need their own state.
3. Read initial values from `window.location.search`.
4. Write state back with `history.replaceState()`.
5. Listen for `popstate` for Back/Forward.
6. Do not put the whole Astro page into React.

## Stretch goals

- Debounce only text search.
- Extract URL logic into a custom hook.
- Add animated chips.
- Add tests for pure filter functions.
- Add a shadcn/ui mobile Sheet.
