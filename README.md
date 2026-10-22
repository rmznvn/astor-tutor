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
