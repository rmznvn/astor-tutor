# Task 19 — Complete reference solution

Use this after your own attempt. Preserve existing project configuration and adapt imports/paths as needed.

## Core implementation
```ts
// vitest.config.ts
/// <reference types="vitest/config" />
import { getViteConfig } from 'astro/config';

export default getViteConfig({
  test: {
    include: ['src/**/*.test.ts'],
  },
});

// src/lib/searchResources.test.ts
import { describe, expect, it } from 'vitest';
import { searchResources } from './searchResources';

describe('searchResources', () => {
  const items = [
    { id: '1', title: 'Astro Routing', description: 'Routes', topic: 'Astro', url: '/1' },
    { id: '2', title: 'CSS Grid', description: 'Layout', topic: 'CSS', url: '/2' },
  ];

  it('matches case-insensitively', () => {
    expect(searchResources(items, 'astro')).toHaveLength(1);
  });

  it('trims whitespace', () => {
    expect(searchResources(items, '  grid  ')).toHaveLength(1);
  });
});

// tests/search.spec.ts
import { test, expect } from '@playwright/test';

test('opens global search with keyboard', async ({ page }) => {
  await page.goto('/');
  await page.keyboard.press(process.platform === 'darwin' ? 'Meta+K' : 'Control+K');
  await expect(page.getByRole('dialog', { name: /search resources/i })).toBeVisible();
});

```

## CSS reference
```css
@layer components {
  .task-19 {
    inline-size: min(100%, 70rem);
    margin-inline: auto;
    padding-inline: clamp(1rem, 4vw, 2rem);
  }

  .task-19 :focus-visible {
    outline: 2px solid currentColor;
    outline-offset: .2rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .task-19 * { scroll-behavior: auto !important; animation: none !important; transition: none !important; }
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
