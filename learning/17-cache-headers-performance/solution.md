# Task 17 — Complete reference solution

Use this after your own attempt. Preserve existing project configuration and adapt imports/paths as needed.

## Core implementation
```ts
// example endpoint cache policy
return new Response(JSON.stringify(payload), {
  headers: {
    'Content-Type': 'application/json; charset=utf-8',
    'Cache-Control': 'public, max-age=300, s-maxage=3600, stale-while-revalidate=86400',
  },
});

// Do NOT use the same public policy for personalized/authenticated responses.
// For private user-specific HTML:
// Cache-Control: private, no-store

```

## CSS reference
```css
@layer components {
  .task-17 {
    inline-size: min(100%, 70rem);
    margin-inline: auto;
    padding-inline: clamp(1rem, 4vw, 2rem);
  }

  .task-17 :focus-visible {
    outline: 2px solid currentColor;
    outline-offset: .2rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .task-17 * { scroll-behavior: auto !important; animation: none !important; transition: none !important; }
}
```

## Integration steps
1. Add the feature to the existing project instead of creating another Astro app.
2. Reuse the shared layout/content model.
3. Run the development flow and test the failure/empty path.
4. Run `npm run build` and, when relevant, `npm run preview`.
5. Inspect HTML, Network, headers, or test output depending on this task.

## Alternative
Try the same requirement with the opposite boundary (client-side fetch vs server rendering, or manual implementation vs Astro-native primitive). Write down which version sends less JS, caches better, and fails more gracefully.
