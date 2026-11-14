# Task 16 — Complete reference solution

Use this after your own attempt. Preserve existing project configuration and adapt imports/paths as needed.

## Core implementation
```ts
// page
---
import RecommendationPanel from '../components/RecommendationPanel.astro';
---
<RecommendationPanel topic="Astro" server:defer>
  <div slot="fallback" class="recommendation-skeleton" aria-label="Loading recommendations">
    Loading recommendations…
  </div>
</RecommendationPanel>

// src/components/RecommendationPanel.astro
---
const { topic } = Astro.props;
const visitor = Astro.cookies.get('visitor-name')?.value ?? 'Developer';
await new Promise((resolve) => setTimeout(resolve, 250));
---
<aside aria-labelledby="personalized-title">
  <h2 id="personalized-title">For {visitor}</h2>
  <p>Continue with more resources about {topic}.</p>
</aside>

```

## CSS reference
```css
@layer components {
  .task-16 {
    inline-size: min(100%, 70rem);
    margin-inline: auto;
    padding-inline: clamp(1rem, 4vw, 2rem);
  }

  .task-16 :focus-visible {
    outline: 2px solid currentColor;
    outline-offset: .2rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .task-16 * { scroll-behavior: auto !important; animation: none !important; transition: none !important; }
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
