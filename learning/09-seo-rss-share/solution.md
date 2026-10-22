# Task 09 — Reference Solution

## 1. Install the integrations

~~~bash
npx astro add sitemap
npm install @astrojs/rss
~~~

## 2. Configure the site

`astro.config.mjs`:

~~~js
import { defineConfig } from 'astro/config';
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: 'https://example.com',
  integrations: [sitemap()],
});
~~~

Replace the example domain with the real deployment URL when you have one.

## 3. Extend the resource schema

~~~ts
schema: ({ image }) =>
  z.object({
    title: z.string(),
    description: z.string(),
    topic: z.string(),
    readingTime: z.number(),
    featured: z.boolean(),
    publishedAt: z.coerce.date(),
    updatedAt: z.coerce.date().optional(),
    cover: image(),
    coverAlt: z.string(),
  }),
~~~

## 4. Base head metadata

`src/components/BaseHead.astro`:

~~~astro
---
type Props = {
  title: string;
  description: string;
  canonical?: URL;
};

const {
  title,
  description,
  canonical = new URL(Astro.url.pathname, Astro.site),
} = Astro.props;
---

<title>{title}</title>
<meta name="description" content={description} />
<link rel="canonical" href={canonical} />

<meta property="og:type" content="article" />
<meta property="og:title" content={title} />
<meta property="og:description" content={description} />
<meta property="og:url" content={canonical} />

<link
  rel="alternate"
  type="application/rss+xml"
  title="Astro Tutor Resources"
  href="/rss.xml"
/>
~~~

## 5. Article JSON-LD

Inside the dynamic resource page:

~~~astro
---
const canonical = new URL(Astro.url.pathname, Astro.site);

const structuredData = {
  '@context': 'https://schema.org',
  '@type': 'Article',
  headline: resource.data.title,
  description: resource.data.description,
  datePublished: resource.data.publishedAt.toISOString(),
  dateModified:
    resource.data.updatedAt?.toISOString() ??
    resource.data.publishedAt.toISOString(),
  url: canonical.toString(),
};
---

<script
  type="application/ld+json"
  set:html={JSON.stringify(structuredData)}
></script>
~~~

## 6. RSS endpoint

Create `src/pages/rss.xml.js`:

~~~js
import rss from '@astrojs/rss';
import { getCollection } from 'astro:content';

export async function GET(context) {
  const resources = await getCollection('resources');

  const sorted = [...resources].sort(
    (a, b) => b.data.publishedAt.valueOf() - a.data.publishedAt.valueOf()
  );

  return rss({
    title: 'Astro Tutor Resources',
    description: 'Practical Astro and frontend learning resources.',
    site: context.site,
    items: sorted.map((resource) => ({
      title: resource.data.title,
      description: resource.data.description,
      pubDate: resource.data.publishedAt,
      link: `/resources/${resource.id}/`,
    })),
  });
}
~~~

## 7. Copy-link component

~~~astro
<div class="share-link" data-share-link>
  <button type="button" data-copy-link>
    Copy link
  </button>

  <span class="share-link__toast" data-copy-status aria-live="polite"></span>
</div>

<script>
  function initCopyLinks() {
    document.querySelectorAll('[data-share-link]').forEach((root) => {
      if (!(root instanceof HTMLElement)) return;
      if (root.dataset.ready === 'true') return;
      root.dataset.ready = 'true';

      const button = root.querySelector('[data-copy-link]');
      const status = root.querySelector('[data-copy-status]');

      if (!(button instanceof HTMLButtonElement)) return;

      button.addEventListener('click', async () => {
        try {
          await navigator.clipboard.writeText(window.location.href);

          if (status) {
            status.textContent = 'Link copied';
          }
        } catch {
          if (status) {
            status.textContent = 'Could not copy the link';
          }
        }
      });
    });
  }

  document.addEventListener('astro:page-load', initCopyLinks);
  initCopyLinks();
</script>
~~~

## 8. CSS architecture

~~~css
@layer reset, tokens, base, components;

@layer tokens {
  :root {
    --surface: #0f1117;
    --text: #f7f8fb;
    --accent: #8b7cff;
    --accent-soft:
      color-mix(in oklab, var(--accent) 24%, transparent);
  }

  @media (prefers-color-scheme: light) {
    :root {
      --surface: #ffffff;
      --text: #15161a;
    }
  }
}

@layer base {
  :where(button, a) {
    font: inherit;
  }
}

@layer components {
  .share-link {
    position: relative;
    display: inline-flex;
    align-items: center;
    gap: .75rem;
  }

  .share-link button {
    border: 1px solid color-mix(in oklab, var(--text) 18%, transparent);
    border-radius: 999px;
    padding-block: .65rem;
    padding-inline: 1rem;
    background: var(--accent-soft);
    color: var(--text);
  }

  .share-link__toast:not(:empty) {
    animation: toast-in 220ms ease both;
  }

  @keyframes toast-in {
    from {
      opacity: 0;
      transform: translateY(.35rem);
    }

    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
}

@media (prefers-reduced-motion: reduce) {
  .share-link__toast {
    animation: none !important;
  }
}
~~~

## Final checks

After `npm run build`, inspect the build output and visit:

~~~text
/rss.xml
/sitemap-0.xml or sitemap.xml (depending on integration output)
~~~

Also inspect View Source on an article and verify the canonical/meta tags are already present.
