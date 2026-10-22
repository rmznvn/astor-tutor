# Task 04 — Responsive Images with astro:assets

## Goal

Upgrade the resource cards and resource detail pages so images are optimized by Astro instead of being treated as ordinary static files.

## You will practice

- `astro:assets`
- `<Image />`
- responsive image layouts
- Content Collection schemas
- Astro's `image()` schema helper
- image metadata in component props
- lazy loading vs priority images
- accessible alt text

## Requirements

1. Add a local cover image to every resource entry.
2. Validate `cover` and `coverAlt` in the content schema.
3. Render covers through Astro's `<Image />`.
4. Use a responsive image layout.
5. The featured card above the fold may load with higher priority.
6. Other cards should remain lazy-loaded.
7. Do not use a decorative image as meaningful content without an alt strategy.

## CSS practice

Use:

- `aspect-ratio`
- `object-fit`
- `object-position`
- overflow clipping
- layered gradients
- container queries
- hover zoom using `transform`
- `prefers-reduced-motion`

## Interaction / decoration

On hover or keyboard focus:

- cover zooms subtly
- overlay becomes slightly brighter
- card title shifts by a few pixels
- no layout shift is allowed

## Acceptance criteria

- Images live under `src/`, not only `public/`.
- Collection schema validates the image field.
- Cards use `<Image />`.
- Generated HTML contains responsive image information.
- Alt text is meaningful.
- Featured image can be prioritized without making every image eager.
- `npm run build` succeeds.

## Hints

1. Collection schemas can receive an `image` helper.
2. The image value returned from content is not just a string path.
3. Astro can generate responsive image behavior when you choose a layout.
4. Prioritize only an image that is genuinely important for initial rendering.

## Stretch goals

- Use `<Picture />` with multiple output formats.
- Add different focal positions per resource.
- Compare Network requests before and after optimization.
- Measure whether the card still avoids CLS.
