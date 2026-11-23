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

### 10 — Astro i18n and bilingual landing page
Add English/Russian routes, a language switcher, typed translation dictionaries, accessible language-aware UI, and localized SEO metadata without changing existing resource routes.


### 11 — Global Search & Command Palette
Build an accessible ⌘K/Ctrl+K search overlay using a minimal Astro Content Collection index and a React island, with keyboard navigation, native dialog, modern CSS, and progressive enhancement.


### 12 — Middleware & Request Context
Use Astro middleware, typed locals, request IDs, safe headers, and development diagnostics.

### 13 — Astro Actions & Feedback Form
Build a validated progressively enhanced feedback form with Astro Actions and accessible error/success states.

### 14 — Typed Environment Configuration
Separate public and server environment variables with a type-safe schema and prevent secret leakage.

### 15 — MDX Component Library
Add MDX and build reusable authoring components for callouts, demos, and documentation content.

### 16 — Server Islands & Personalized UI
Use `server:defer` to isolate personalized request-time UI while keeping the surrounding page fast and cacheable.

### 17 — Caching, Headers & Performance Budget
Design cache policies for static/data/dynamic responses and practice rendering-performance techniques.

### 18 — 404, Error States & Resilient UX
Build recovery-oriented 404 and empty/error states with high-contrast and accessibility testing.

### 19 — Testing with Vitest & Playwright
Add unit and end-to-end tests for search logic, keyboard interaction, 404 behavior, and mobile overflow.

### 20 — URL-synced Reading List
Build a shareable, accessible Read Later tray using Astro HTML, localStorage, URLSearchParams, ClientRouter lifecycle, and container queries.

### 21 — URL-first Faceted Resource Filters
Build shareable topic and difficulty filters on the resource library using GET form controls and URLSearchParams.

### 22 — Related Content & Backlinks
Derive a build-time relationship graph from the existing content collection and render related articles and incoming references.

### 23 — Accessible Screenshot Gallery
Create a reusable Astro gallery with optimized thumbnails and a native dialog viewer, keyboard controls and focus restoration.


### 24 — Downloadable Event Calendar Feed
Generate an accessible .ics calendar download from typed events with an Astro static endpoint and responsive CSS Grid.

### 25 — Accessible Reading Progress & Section Bookmarks
Add native progress semantics and shareable heading anchors to articles, with ClientRouter-safe event handling and modern scrolling CSS.

### 26 — Build-time Content Quality Audit
Audit collection-wide metadata and relationships with a pure TypeScript validator, Astro build artifact, CI-friendly checker and container-query checklist.

### 27 — Accessible Code Snippet Toolbar
Documentation readers need copyable code examples with visible language labels, a confirmation message and robust navigation when Astro ClientRouter swaps pages.

### 28 — Responsive Feature Comparison from Content
A frontend knowledge hub needs a comparison matrix for Astro, React and Webflow techniques, authored in typed content and usable on narrow screens.

### 29 — Offline Reading Pack with Service Worker
Users want to save a small set of already-public article pages for offline reading without converting the Astro site into a full PWA.

## Rule

Try the task before opening `solution.md`.

Each task practices Astro plus a different set of modern CSS, interaction, accessibility, architecture, and performance techniques.
