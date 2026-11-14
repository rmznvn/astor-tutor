# Task 18 — Complete reference solution

Use this after your own attempt. Preserve existing project configuration and adapt imports/paths as needed.

## Core implementation
```ts
// src/pages/404.astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---
<BaseLayout title="Page not found">
  <main class="not-found">
    <p class="not-found__code">404</p>
    <h1>Page not found</h1>
    <p>The page may have moved or the link may be incorrect.</p>
    <nav aria-label="Recovery links">
      <a href="/">Home</a>
      <a href="/resources">Browse resources</a>
      <a href="/resources#search">Search resources</a>
    </nav>
  </main>
</BaseLayout>

// src/components/EmptyState.astro
---
const { title, message } = Astro.props;
---
<section class="empty-state" aria-labelledby="empty-title">
  <h2 id="empty-title">{title}</h2>
  <p>{message}</p>
  <slot />
</section>

```

## CSS reference
```css
@layer components {
  .task-18 {
    inline-size: min(100%, 70rem);
    margin-inline: auto;
    padding-inline: clamp(1rem, 4vw, 2rem);
  }

  .task-18 :focus-visible {
    outline: 2px solid currentColor;
    outline-offset: .2rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .task-18 * { scroll-behavior: auto !important; animation: none !important; transition: none !important; }
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
