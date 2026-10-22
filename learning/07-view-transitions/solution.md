# Task 07 — Reference Solution

## 1. Enable client routing in the shared head

Example `BaseHead.astro`:

~~~astro
---
import { ClientRouter } from 'astro:transitions';
---

<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width" />
  <ClientRouter />
  <slot />
</head>
~~~

If your project already owns the `<head>` elsewhere, add `<ClientRouter />` there instead.

## 2. Match a card element with the detail page

In the card:

~~~astro
<h2 transition:name={`resource-title-${resource.id}`}>
  <a href={`/resources/${resource.id}`}>
    {resource.data.title}
  </a>
</h2>
~~~

In `[slug].astro`:

~~~astro
<h1 transition:name={`resource-title-${resource.id}`}>
  {resource.data.title}
</h1>
~~~

## 3. Make article scripts lifecycle-aware

Instead of assuming one full page load:

~~~astro
<script>
  let cleanup = () => {};

  function initArticleUi() {
    cleanup();

    const progress = document.querySelector('[data-progress]');
    const article = document.querySelector('[data-article]');

    if (!(progress instanceof HTMLElement)) return;
    if (!(article instanceof HTMLElement)) return;

    const updateProgress = () => {
      const rect = article.getBoundingClientRect();
      const start = window.scrollY + rect.top;
      const max = Math.max(article.offsetHeight - window.innerHeight, 1);
      const travelled = Math.max(window.scrollY - start, 0);
      const ratio = Math.min(travelled / max, 1);

      progress.style.setProperty('--progress', `${ratio * 100}%`);
    };

    updateProgress();
    window.addEventListener('scroll', updateProgress, { passive: true });

    cleanup = () => {
      window.removeEventListener('scroll', updateProgress);
    };
  }

  document.addEventListener('astro:page-load', initArticleUi);
  initArticleUi();
</script>
~~~

The cleanup step prevents duplicate listeners after repeated navigation.

## 4. Transition styling

~~~css
::view-transition-old(root),
::view-transition-new(root) {
  animation-duration: 260ms;
  animation-timing-function: cubic-bezier(.2, .8, .2, 1);
}

.resource-card a {
  position: relative;
  text-decoration: none;
}

.resource-card a::after {
  content: "";
  position: absolute;
  inset-inline: 0;
  inset-block-end: -.18em;
  block-size: 1px;
  background: currentColor;
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 220ms ease;
}

.resource-card a:is(:hover, :focus-visible)::after {
  transform: scaleX(1);
  transform-origin: left;
}

@media (prefers-reduced-motion: reduce) {
  ::view-transition-group(*),
  ::view-transition-old(*),
  ::view-transition-new(*) {
    animation-duration: 0.001ms !important;
  }

  .resource-card a::after {
    transition: none;
  }
}
~~~

## Test checklist

Navigate:

~~~text
/resources
→ article A
→ back
→ article B
→ article C
→ back twice
~~~

Then verify the reading progress listener is not firing multiple times.
