# Task 12 — Complete reference solution

This is a reference implementation. Adapt file paths to your current project and keep existing integrations/configuration.

## Core implementation

```ts
// src/middleware.ts
import { defineMiddleware } from 'astro:middleware';

export const onRequest = defineMiddleware(async (context, next) => {
  const requestId = context.request.headers.get('x-request-id') ?? crypto.randomUUID();
  context.locals.requestId = requestId;
  const response = await next();
  response.headers.set('x-request-id', requestId);
  return response;
});

// src/env.d.ts
declare namespace App {
  interface Locals {
    requestId: string;
  }
}

```

## Integration checklist
1. Add the new file(s) without deleting previous task code.
2. Wire the feature into the shared layout or relevant route.
3. Add the CSS practice from `task.md` in a scoped component stylesheet or your existing layered stylesheet.
4. Verify keyboard/focus behavior and reduced motion.
5. Run `npm run build` and `npm run preview`.

## CSS reference direction
```css
@layer components {
  .task-12 {
    container-type: inline-size;
    padding-block: clamp(1rem, 3vw, 2rem);
    padding-inline: clamp(1rem, 4vw, 2.5rem);
  }

  .task-12:focus-within {
    outline: 1px solid color-mix(in oklab, currentColor 25%, transparent);
    outline-offset: .25rem;
  }
}

@media (prefers-reduced-motion: reduce) {
  .task-12 * { animation-duration: 0.001ms !important; transition-duration: 0.001ms !important; }
}
```

## Alternative
Implement the same business requirement with a custom API endpoint/client script, then compare complexity, payload, validation, and failure modes. Keep the Astro-native version unless the custom approach gives a concrete product benefit.

## Final verification
- View generated HTML where relevant.
- Inspect Network and client bundles.
- Test with JavaScript disabled if the feature has a meaningful fallback.
- Confirm no secrets or unnecessary data appear in page source.
