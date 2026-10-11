# Astro Tutor

Practice repository for learning Astro through realistic frontend features.

## Goal

Build small features that resemble daily Astro work while practicing modern CSS, UI interaction, accessibility, performance, React islands, routing, and content collections.

## How to use this repo

1. Open the next task in `learning/`.
2. Read only the Task, Requirements, Acceptance Criteria, and Hints first.
3. Build the feature yourself.
4. Commit your attempt on a feature branch.
5. Then open the Explanation and Solution sections to compare approaches.
6. Refactor only after you understand why the solution works.

## Roadmap

- `01-content-collection` — reusable resource section from an Astro Content Collection
- `02-dynamic-routes` — dynamic resource pages, sticky TOC, and reading progress
- `03-react-filtering` — filterable resource library with a React island and URL state

## Local setup

~~~bash
npm install
npm run dev
~~~

Then open the local URL shown by Astro.

## Suggested Git workflow

Create a branch per task:

~~~bash
git switch -c task/01-content-collection
~~~

After finishing:

~~~bash
git add .
git commit -m "Complete task 01 content collection"
git push -u origin task/01-content-collection
~~~

Then open a pull request into `main`.