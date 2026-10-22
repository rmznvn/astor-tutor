# Task 08 — Reference Solution

## 1. Create `src/pages/api/resources.json.ts`

~~~ts
import type { APIRoute } from 'astro';
import { getCollection } from 'astro:content';

export const GET: APIRoute = async () => {
  const resources = await getCollection('resources');

  const payload = resources.map((resource) => ({
    id: resource.id,
    title: resource.data.title,
    description: resource.data.description,
    topic: resource.data.topic,
    readingTime: resource.data.readingTime,
    url: `/resources/${resource.id}`,
  }));

  return new Response(JSON.stringify(payload), {
    headers: {
      'Content-Type': 'application/json; charset=utf-8',
      'Cache-Control': 'public, max-age=3600',
    },
  });
};
~~~

## 2. Create `src/components/SurpriseResource.astro`

~~~astro
<div class="surprise" data-surprise>
  <button type="button" data-surprise-trigger>
    Surprise me
  </button>

  <p class="surprise__status" data-surprise-status aria-live="polite"></p>

  <dialog class="surprise-dialog" data-surprise-dialog>
    <form method="dialog" class="surprise-dialog__close">
      <button aria-label="Close preview">×</button>
    </form>

    <p data-surprise-topic></p>
    <h2 data-surprise-title></h2>
    <p data-surprise-description></p>
    <a data-surprise-link>Open resource →</a>
  </dialog>
</div>

<script>
  function initSurpriseResource() {
    document.querySelectorAll('[data-surprise]').forEach((root) => {
      if (!(root instanceof HTMLElement)) return;
      if (root.dataset.ready === 'true') return;
      root.dataset.ready = 'true';

      const trigger = root.querySelector('[data-surprise-trigger]');
      const status = root.querySelector('[data-surprise-status]');
      const dialog = root.querySelector('[data-surprise-dialog]');
      const topic = root.querySelector('[data-surprise-topic]');
      const title = root.querySelector('[data-surprise-title]');
      const description = root.querySelector('[data-surprise-description]');
      const link = root.querySelector('[data-surprise-link]');

      if (!(trigger instanceof HTMLButtonElement)) return;
      if (!(dialog instanceof HTMLDialogElement)) return;

      trigger.addEventListener('click', async () => {
        trigger.disabled = true;
        if (status) status.textContent = 'Finding a resource…';

        try {
          const response = await fetch('/api/resources.json');

          if (!response.ok) {
            throw new Error('Request failed');
          }

          const resources = await response.json();
          const resource =
            resources[Math.floor(Math.random() * resources.length)];

          if (!resource) throw new Error('No resources available');

          if (topic) topic.textContent = resource.topic;
          if (title) title.textContent = resource.title;
          if (description) description.textContent = resource.description;

          if (link instanceof HTMLAnchorElement) {
            link.href = resource.url;
          }

          if (status) status.textContent = '';
          dialog.showModal();
        } catch {
          if (status) {
            status.textContent = 'Could not load a resource. Try again.';
          }
        } finally {
          trigger.disabled = false;
        }
      });
    });
  }

  document.addEventListener('astro:page-load', initSurpriseResource);
  initSurpriseResource();
</script>
~~~

## 3. CSS

~~~css
.surprise-dialog {
  inline-size: min(32rem, calc(100% - 2rem));
  border: 1px solid color-mix(in oklab, currentColor 18%, transparent);
  border-radius: 1.25rem;
  padding: clamp(1.25rem, 4vw, 2rem);
  background: #111319;
  color: white;
}

.surprise-dialog::backdrop {
  background:
    radial-gradient(circle at 50% 20%, rgb(100 80 220 / .18), transparent 35%),
    rgb(0 0 0 / .72);
  backdrop-filter: blur(6px);
}

.surprise-dialog__close {
  position: absolute;
  inset-block-start: .75rem;
  inset-inline-end: .75rem;
}

.surprise:has([data-surprise-trigger]:disabled) {
  opacity: .75;
}

.surprise button:focus-visible,
.surprise-dialog a:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 3px;
}

@media (prefers-reduced-motion: reduce) {
  .surprise-dialog,
  .surprise-dialog::backdrop {
    animation: none;
  }
}
~~~

## Architectural note

The endpoint is deliberately small. Your page components should still query Content Collections directly when they render on the server/build side. The endpoint exists only for browser-side consumers that actually need HTTP data.
