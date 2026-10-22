# Task 08 — Build a Resource JSON Endpoint and “Surprise Me” Feature

## Goal

Expose a lightweight JSON representation of your resource collection and consume it from a small browser interaction.

The user clicks **Surprise me**, the browser requests the JSON endpoint, selects a random resource, and opens a preview dialog.

## You will practice

- Astro static endpoints
- exporting a `GET()` handler
- returning a `Response`
- selecting safe public fields from collection data
- client/server boundaries
- `fetch()`
- native `<dialog>`
- loading/error states

## Required route

~~~text
/api/resources.json
~~~

The endpoint should return only fields the browser needs, for example:

- id
- title
- description
- topic
- readingTime
- url

Do not serialize the entire collection entry blindly.

## Interaction

Create a `SurpriseResource.astro` component:

1. User presses the button.
2. Button shows a loading state.
3. Browser fetches `/api/resources.json`.
4. Pick a random resource.
5. Open a native dialog with resource info.
6. Include a real link to the resource.
7. Handle request failure visibly.

## CSS practice

Use:

- `::backdrop`
- `color-mix()`
- logical inset/padding properties
- `:focus-visible`
- `:has()`
- discrete open/close transition where supported
- reduced-motion fallback

## Acceptance criteria

- Opening `/api/resources.json` returns valid JSON.
- The endpoint contains no unnecessary content/body/image metadata.
- Button prevents accidental repeated loading clicks.
- Dialog is keyboard usable.
- Error state is announced.
- Feature still works after Task 07 client-side navigation.
- Build succeeds.

## Hints

- Static endpoints can generate non-HTML files at build time.
- Use `getCollection()` inside the endpoint.
- Use `response.ok` before trusting the response.
- Native dialog gives you focus management and a backdrop for free.

## Stretch goals

- Avoid choosing the same resource twice in a row.
- Add a topic filter before random selection.
- Add an endpoint version field.
