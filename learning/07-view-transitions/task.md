# Task 07 — Smooth Resource Navigation with Astro View Transitions

## Goal

Make navigation from a resource card to its detail page feel polished without converting the site into a large React SPA.

## You will practice

- Astro's `ClientRouter`
- shared view-transition names
- navigation lifecycle events
- `astro:page-load`
- re-initializing page scripts correctly
- progressive enhancement
- reduced-motion support

## Requirements

1. Add Astro's client router in the shared head/layout.
2. Give the clicked resource title or cover a matching transition name on list and detail pages.
3. Keep navigation as normal links.
4. Make the reading-progress/TOC script from Task 02 work after client-side navigation.
5. Do not rely on page-load code that only runs once.
6. Verify browser Back/Forward navigation.
7. Respect `prefers-reduced-motion`.

## CSS practice

Use:

- `::view-transition-old()`
- `::view-transition-new()`
- custom easing
- `@media (prefers-reduced-motion: reduce)`
- subtle link underline animation with pseudo-elements

## Acceptance criteria

- Navigation works with JavaScript disabled as ordinary links.
- With client routing enabled, navigation animates.
- Reading progress still works after visiting multiple articles.
- Back/Forward does not duplicate event listeners.
- Focus remains usable.
- Build succeeds.

## Hints

- A bundled Astro script may only execute once when client routing is active.
- Listen for Astro's page lifecycle instead of assuming a full reload.
- Keep initialization idempotent or clean up previous listeners.
- Shared transition names should be unique per resource.

## Stretch goals

- Add a navigation progress indicator.
- Preserve a small piece of UI state between pages.
- Compare native cross-document transitions with `ClientRouter`.
