# Astro Tutor

Practice repository for learning Astro through realistic frontend features.

## Goal

Build one evolving Astro project while practicing production-style functionality, modern CSS, UI interaction, accessibility, performance, React islands, routing, content architecture, and deployment-oriented features.

## Learning structure

Each task lives in `learning/` and contains:

```text
README.md
task.md
explanation.md
solution.md
```

Work in this order:

1. Read `task.md`.
2. Build the feature yourself in `src/`.
3. Commit your attempt.
4. Read `explanation.md`.
5. Compare your implementation with `solution.md`.
6. Refactor only after you understand the differences.

## Current roadmap

- 01 Content Collection
- 02 Dynamic Routes
- 03 React Filtering
- 04 Responsive Images
- 05 Slots and Component API
- 06 Static Pagination
- 07 View Transitions
- 08 JSON Endpoint
- 09 SEO, RSS, Sitemap, and Share UX
- 10 Astro i18n and bilingual landing page

- 11 Global Search & Command Palette
- 12 Middleware & Request Context
- 13 Astro Actions & Feedback Form
- 14 Typed Environment Configuration
- 15 MDX Component Library
- 16 Server Islands & Personalized UI
- 17 Caching, Headers & Performance Budget
- 18 404, Error States & Resilient UX
- 19 Testing with Vitest & Playwright

- 21 URL-first Faceted Resource Filters
- 22 Related Content & Backlinks
- 23 Accessible Screenshot Gallery

- 24 Downloadable Event Calendar Feed
- 25 Accessible Reading Progress & Section Bookmarks
- 26 Build-time Content Quality Audit
- 27 Accessible Code Snippet Toolbar
- 28 Responsive Feature Comparison from Content
- 29 Offline Reading Pack with Service Worker

- 30 Typed Release Notes & Native Disclosure
- 31 Markdown External Link Policy
- 32 Custom Astro Integration & Build Metrics

New tasks continue the same project rather than creating a new Astro app.

## Local setup

```bash
npm install
npm run dev
```

Before considering a task complete:

```bash
npm run build
```

## Suggested Git workflow

Create a branch per task:

```bash
git switch main
git pull
git switch -c task/04-responsive-images
```

After finishing:

```bash
git add .
git commit -m "Complete task 04 responsive images"
git push -u origin task/04-responsive-images
```

Then open a pull request into `main`.


### Task 20 — URL-synced Reading List
See [learning/20-url-synced-reading-list](learning/20-url-synced-reading-list) for a shareable reading-list exercise.
