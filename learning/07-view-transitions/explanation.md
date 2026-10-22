# Task 07 — Explanation

## Why this is an Astro-specific problem

A traditional multi-page navigation reloads the whole document. Many scripts naturally re-run.

When `ClientRouter` intercepts navigation, Astro can swap page content without a full reload. That means code written with the assumption:

> this script runs every time the page opens

may stop being true.

## Mental model

~~~text
normal link
   ↓
ClientRouter intercepts
   ↓
new document prepared
   ↓
DOM swapped
   ↓
astro:page-load
   ↓
page-specific behavior initialized
~~~

## Why this matters in real projects

Adding smooth transitions late in a project can reveal hidden assumptions in:

- analytics
- sliders
- scroll animations
- table-of-contents code
- third-party widgets
- event listeners

The transition itself is easy. Correct lifecycle handling is the valuable skill.

## Progressive enhancement

Links must remain real `<a href>` elements.

If JavaScript fails or transitions are unsupported, navigation should still work.

## Interview questions

### Why can a script break after adding ClientRouter?

A strong answer:

Client-side navigation can replace page content without a full browser reload, while bundled module scripts may not execute again. Page-specific initialization should respond to Astro's navigation lifecycle.

### Why not use React Router?

A strong answer:

Astro already provides file-based routing and can enhance document navigation. Adding a separate SPA router would duplicate routing responsibilities for a content-focused site.

### What is a shared element transition?

A strong answer:

The browser visually connects matching elements in the old and new document states, creating continuity between views rather than animating two unrelated full pages.
