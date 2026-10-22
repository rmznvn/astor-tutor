# Learning roadmap

These tasks are intentionally cumulative.

## How each task is organized

Every task folder contains:

```text
README.md
task.md
explanation.md
solution.md
```

Use them in this order:

1. `task.md` — build it yourself first.
2. `explanation.md` — understand the architecture and concepts.
3. `solution.md` — compare your work with the complete reference solution.

The working Astro application stays in `src/`. The `learning/` folder is your study material and reference.

## Task order

### 01 — Content Collection
Build a reusable Latest Resources section from local content entries.

### 02 — Dynamic Routes
Turn those resources into real detail pages with a sticky TOC, reading progress, and Previous/Next navigation.

### 03 — React Filtering
Build a filterable resource library and use React only for the interactive state.

### 04 — Responsive Images
Use `astro:assets`, collection image schemas, responsive images, loading priority, and performance-friendly image UI.

### 05 — Slots and Component API
Design a reusable section component with default/named slots, optional wrappers, semantic composition, and modern CSS.

### 06 — Static Pagination
Build a paginated archive with `paginate()`, catch-all routes, accessible navigation, and progressive CSS animation.

### 07 — View Transitions
Add `ClientRouter`, shared transitions, and correct page lifecycle handling without turning the project into an SPA.

### 08 — JSON Endpoint
Create a static JSON endpoint from the collection and a browser-side “Surprise me” feature with native dialog UI.

### 09 — SEO, RSS, Sitemap, and Share UX
Add production metadata, canonical URLs, JSON-LD, RSS, sitemap generation, and a lightweight Copy Link interaction.

## Rule

Try the task before opening `solution.md`.

Each task practices Astro plus a different set of modern CSS, interaction, accessibility, architecture, and performance techniques.
