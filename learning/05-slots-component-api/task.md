# Task 05 — Build a Reusable Section API with Slots

## Goal

Create a reusable `ContentSection.astro` component that can power several visually related sections without hard-coding their content.

Use it for at least:

- Latest Resources
- Featured Topics
- Continue Learning

## You will practice

- default slots
- named slots
- `Astro.props`
- `Astro.slots.has()`
- component composition
- conditional markup
- semantic section headings
- avoiding over-specific components

## Required component API

Your component should support:

- `title`
- optional `description`
- optional `id`
- optional visual variant
- named `eyebrow` slot
- default content slot
- optional `actions` slot
- optional `footer` slot

## CSS practice

Use:

- CSS logical properties
- CSS custom properties
- `subgrid`
- `:where()`
- `:has()`
- fluid spacing with `clamp()`

## UI interaction

Add a small expandable `<details>` block to one section. Style its open state with CSS only.

## Acceptance criteria

- The same component is reused at least three times.
- No component checks for a specific page name.
- Optional slots do not render empty wrappers.
- Heading hierarchy remains valid.
- Layout works in narrow and wide containers.
- No JavaScript is required for the disclosure.
- `npm run build` succeeds.

## Hints

- Use `Astro.slots.has('actions')` before rendering the actions wrapper.
- Named slots belong to direct child elements passed into the component.
- A reusable component should expose meaningful variation, not every possible CSS property.

## Stretch goals

- Add an `as`-style semantic option using a small controlled set of elements.
- Make section cards align with `subgrid`.
- Add a compact variant without duplicating markup.
