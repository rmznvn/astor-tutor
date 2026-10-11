# Task 01 — Reusable Content Section

## Goal

Build a `Latest Resources` section powered by an Astro Content Collection.

The page must render resource cards from content files instead of hard-coded HTML.

## You will practice

- `content.config.ts`
- `defineCollection()`
- `glob()`
- `getCollection()`
- Astro component props
- `.map()`
- conditional classes
- reusable components
- content vs presentation separation

## Required structure

~~~text
src/
├── content/
│   └── resources/
│       ├── astro-routing.md
│       ├── css-container-queries.md
│       └── array-methods.md
├── components/
│   ├── ResourceCard.astro
│   └── ResourcesGrid.astro
└── content.config.ts
~~~

Each resource must have:

- title
- description
- topic
- reading time
- featured boolean

## UI

Show a responsive grid of cards.

Each card should display:

- topic
- title
- description
- reading time
- arrow/action affordance

The featured card should look different without creating a second card component.

## CSS practice

Use:

- CSS Grid
- `repeat(auto-fit, minmax(...))`
- `clamp()`
- `aspect-ratio`
- container queries
- pseudo-elements
- subtle gradient decoration
- `:focus-visible`
- `prefers-reduced-motion`

## Interaction

On hover/focus:

- card lifts slightly
- gradient/border becomes more visible
- arrow moves a few pixels
- media area scales slightly

Prefer `transform` and `opacity`.

## Acceptance criteria

- No resource is hard-coded in `index.astro`.
- Data comes from `getCollection('resources')`.
- `ResourceCard.astro` receives one resource through props.
- Adding a fourth Markdown file creates a fourth card automatically.
- Featured state uses the same card component.
- Keyboard focus is visible.
- Layout works on mobile and desktop.
- `npm run build` succeeds.

## Hints

1. Configure the collection before styling anything.
2. Temporarily log the result of `getCollection('resources')` to inspect its shape.
3. Remember: collection fields are under `entry.data`.
4. Keep the card component focused on one item.
5. Use `container-type: inline-size` on the card if you want it to respond to its own width.

## Stretch goals

- Sort featured items first.
- Add `publishedAt` and sort newest first.
- Make the entire card clickable accessibly.
- Create a compact card variant using container queries only.
