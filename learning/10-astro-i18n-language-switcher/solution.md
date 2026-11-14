# Task 10 — Complete reference solution

This is a **reference implementation**, not code that has already been installed in `src/`. Implement it on your own task branch after reading `task.md`.

**Assumption:** You completed Task 01 and have a `resources` Content Collection with `title`, `description`, and `topic` fields. The articles themselves remain English. The new landing pages are separate from the React-filtered resource library.

## 1. Update `astro.config.mjs`

Add the `i18n` property to your **existing** config; keep the `site` and integrations added in Task 09.

```js
import { defineConfig } from 'astro/config';

// If you already have integrations, keep their imports and configuration.
export default defineConfig({
  site: 'https://example.com', // Replace with your real deployment URL.
  i18n: {
    locales: ['en', 'ru'],
    defaultLocale: 'en',
    routing: {
      prefixDefaultLocale: false,
    },
  },
});
```

**Important:** This shows the relevant config shape, not a reason to remove existing `integrations: [...] ` or other options. English routes stay unprefixed.

## 2. Create `src/i18n/ui.ts`

```ts
export const ui = {
  en: {
    pageTitle: 'Start learning Astro',
    intro: 'Explore practical frontend resources and build your skills.',
    eyebrow: 'Astro Tutor',
    resourcesHeading: 'Start with these resources',
    openArticle: 'Read article',
    englishArticle: 'Full article is in English',
    untranslatedSummary: 'Summary currently available in English',
    empty: 'No resources yet. Complete Task 01 to add your first entries.',
    switcherLabel: 'Choose language',
  },
  ru: {
    pageTitle: 'Начните изучать Astro',
    intro: 'Изучайте практические материалы и развивайте навыки фронтенд-разработки.',
    eyebrow: 'Astro Tutor',
    resourcesHeading: 'С чего начать',
    openArticle: 'Читать статью',
    englishArticle: 'Полная статья доступна на английском',
    untranslatedSummary: 'Описание пока доступно на английском',
    empty: 'Материалов пока нет. Сначала выполните задание 01.',
    switcherLabel: 'Выбор языка',
  },
} as const;

export type Locale = keyof typeof ui;
```

This object is checked by TypeScript. The page component can accept only `'en'` or `'ru'`.

## 3. Create `src/i18n/resourceSummaries.ts`

These are **optional Russian summaries**, keyed by stable resource IDs. Replace IDs or translations to match the entries you actually created in Task 01.

```ts
import type { CollectionEntry } from 'astro:content';
import type { Locale } from './ui';

type Summary = {
  title: string;
  description: string;
};

const russianSummaries: Record<string, Summary> = {
  'astro-routing': {
    title: 'Маршрутизация в Astro',
    description: 'Разберитесь, как Astro создаёт URL из файлов и когда нужны динамические маршруты.',
  },
  'css-container-queries': {
    title: 'Контейнерные запросы CSS',
    description: 'Создавайте компоненты, которые адаптируются к доступному пространству.',
  },
};

export function getResourceSummary(
  entry: CollectionEntry<'resources'>,
  locale: Locale
) {
  const translation =
    locale === 'ru' ? russianSummaries[entry.id] : undefined;

  return {
    title: translation?.title ?? entry.data.title,
    description: translation?.description ?? entry.data.description,
    isTranslated: locale === 'en' || Boolean(translation),
  };
}
```

A missing Russian summary is allowed, but must be disclosed in the UI.

## 4. Create `src/components/LanguageSwitcher.astro`

```astro
---
import { getRelativeLocaleUrl } from 'astro:i18n';
import { ui, type Locale } from '../i18n/ui';

type Props = { locale: Locale };
const { locale } = Astro.props;

const languages = [
  { code: 'en', label: 'English' },
  { code: 'ru', label: 'Русский' },
] as const;
---

<nav class="language-switcher" aria-label={ui[locale].switcherLabel}>
  {languages.map((language) => (
    <a
      href={getRelativeLocaleUrl(language.code, 'start')}
      lang={language.code}
      hreflang={language.code}
      aria-current={language.code === locale ? 'page' : undefined}
      class="language-switcher__link"
    >
      {language.label}
    </a>
  ))}
</nav>

<style>
  .language-switcher {
    display: inline-flex;
    flex-wrap: wrap;
    gap: .75rem;
    align-items: center;
  }

  .language-switcher__link {
    position: relative;
    display: inline-block;
    padding-block: .55rem;
    padding-inline: .2rem;
    color: inherit;
    text-decoration: none;
    opacity: .7;
    transition: opacity 180ms ease;
  }

  .language-switcher__link::after {
    content: '';
    position: absolute;
    inset-inline: 0;
    inset-block-end: .25rem;
    block-size: 2px;
    background: currentColor;
    transform: scaleX(0);
    transform-origin: center;
    transition: transform 180ms ease;
  }

  .language-switcher__link:is(:hover, [aria-current='page']) {
    opacity: 1;
  }

  .language-switcher__link:is(:hover, [aria-current='page'])::after {
    transform: scaleX(1);
  }

  .language-switcher__link:focus-visible {
    outline: 2px solid currentColor;
    outline-offset: 4px;
    border-radius: .2rem;
  }

  @media (prefers-reduced-motion: reduce) {
    .language-switcher__link,
    .language-switcher__link::after {
      transition: none;
    }
  }
</style>
```

The active link has `aria-current="page"`; the language names are pronounced correctly because each link has its own `lang`.

## 5. Create `src/components/LocalizedStartPage.astro`

This shared page renders **both** language routes and fetches the existing Content Collection at build time.

```astro
---
import { getCollection } from 'astro:content';
import { getRelativeLocaleUrl } from 'astro:i18n';
import { ClientRouter } from 'astro:transitions';
import LanguageSwitcher from './LanguageSwitcher.astro';
import { ui, type Locale } from '../i18n/ui';
import { getResourceSummary } from '../i18n/resourceSummaries';

type Props = { locale: Locale };
const { locale } = Astro.props;
const copy = ui[locale];

const resources = (await getCollection('resources'))
  .sort((a, b) => a.data.title.localeCompare(b.data.title))
  .slice(0, 3);

const enPath = getRelativeLocaleUrl('en', 'start');
const ruPath = getRelativeLocaleUrl('ru', 'start');
const currentPath = getRelativeLocaleUrl(locale, 'start');

// Task 09 should already configure a real site URL.
// Astro.url is only a local-development fallback.
const base = Astro.site ?? Astro.url;
const absolute = (path: string) => new URL(path, base).toString();
---

<!doctype html>
<html lang={locale}>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <title>{copy.pageTitle} | Astro Tutor</title>
    <meta name="description" content={copy.intro} />
    <link rel="canonical" href={absolute(currentPath)} />
    <link rel="alternate" hreflang="en" href={absolute(enPath)} />
    <link rel="alternate" hreflang="ru" href={absolute(ruPath)} />

    <ClientRouter />
  </head>

  <body>
    <main class="start-page">
      <header class="start-page__header">
        <a class="start-page__home" href="/">
          {copy.eyebrow}
        </a>

        <LanguageSwitcher locale={locale} />
      </header>

      <section class="start-page__hero" aria-labelledby="start-title">
        <p class="start-page__eyebrow">{copy.eyebrow}</p>
        <h1 id="start-title">{copy.pageTitle}</h1>
        <p class="start-page__intro">{copy.intro}</p>
      </section>

      <section aria-labelledby="start-resources">
        <h2 id="start-resources">{copy.resourcesHeading}</h2>

        {resources.length > 0 ? (
          <ul class="resource-grid">
            {resources.map((entry) => {
              const summary = getResourceSummary(entry, locale);

              return (
                <li class="resource-card">
                  <article>
                    <p class="resource-card__topic">{entry.data.topic}</p>
                    <h3>{summary.title}</h3>
                    <p>{summary.description}</p>

                    {!summary.isTranslated && (
                      <p class="resource-card__notice">
                        {copy.untranslatedSummary}
                      </p>
                    )}

                    <p class="resource-card__notice">
                      {copy.englishArticle}
                    </p>

                    <a
                      class="resource-card__link"
                      href={getRelativeLocaleUrl('en', `resources/${entry.id}`)}
                      hreflang="en"
                      lang="en"
                    >
                      {copy.openArticle}
                      <span aria-hidden="true">↗</span>
                    </a>
                  </article>
                </li>
              );
            })}
          </ul>
        ) : (
          <p>{copy.empty}</p>
        )}
      </section>
    </main>
  </body>
</html>

<style>
  :global(html) {
    color-scheme: dark;
    background: #0e1119;
    color: #f5f7fb;
  }

  :global(body) {
    margin: 0;
    font-family: system-ui, sans-serif;
    line-height: 1.5;
  }

  .start-page {
    inline-size: min(72rem, calc(100% - 2rem));
    margin-inline: auto;
    padding-block: clamp(1.25rem, 3vw, 2rem) clamp(4rem, 9vw, 8rem);
  }

  .start-page__header {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
    gap: 1rem;
  }

  .start-page__home {
    color: inherit;
    font-weight: 700;
    text-decoration: none;
  }

  .start-page__home:focus-visible,
  .resource-card__link:focus-visible {
    outline: 2px solid #c5bcff;
    outline-offset: 4px;
  }

  .start-page__hero {
    max-inline-size: 50rem;
    padding-block: clamp(4rem, 10vw, 8rem);
  }

  .start-page__eyebrow {
    letter-spacing: .12em;
    text-transform: uppercase;
    color: #b9b0ff;
  }

  h1 {
    margin-block: .5rem 1.5rem;
    font-size: clamp(2.5rem, 8vw, 5.5rem);
    line-height: 1.05;
    text-wrap: balance;
  }

  .start-page__intro {
    max-inline-size: 46ch;
    font-size: clamp(1.125rem, 2vw, 1.4rem);
    text-wrap: pretty;
  }

  .resource-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(min(100%, 18rem), 1fr));
    gap: 1rem;
    margin: 1.5rem 0 0;
    padding: 0;
    list-style: none;
  }

  .resource-card {
    min-inline-size: 0;
    border: 1px solid rgb(255 255 255 / .14);
    border-radius: 1rem;
    padding: clamp(1.25rem, 3vw, 2rem);
    background: linear-gradient(150deg, #202538, #151925);
    transition: transform 200ms ease, border-color 200ms ease;
  }

  .resource-card:is(:hover, :focus-within) {
    transform: translateY(-3px);
    border-color: #b9b0ff;
  }

  .resource-card h3 {
    margin-block: .5rem 1rem;
    font-size: clamp(1.25rem, 2vw, 1.6rem);
    text-wrap: balance;
    overflow-wrap: anywhere;
  }

  .resource-card p {
    text-wrap: pretty;
  }

  .resource-card__topic {
    color: #b9b0ff;
  }

  .resource-card__notice {
    font-size: .85rem;
    color: #bfc6d6;
  }

  .resource-card__link {
    display: inline-flex;
    align-items: center;
    gap: .5rem;
    margin-block-start: 1rem;
    color: #d7d0ff;
    text-underline-offset: .25em;
  }

  /* Language-aware typography: a useful localization-specific CSS tool. */
  :global(html:lang(ru)) .start-page__intro,
  :global(html:lang(ru)) .resource-card h3,
  :global(html:lang(ru)) .resource-card p {
    hyphens: auto;
    overflow-wrap: anywhere;
  }

  @media (prefers-reduced-motion: reduce) {
    .resource-card {
      transition: none;
    }

    .resource-card:is(:hover, :focus-within) {
      transform: none;
    }
  }
</style>
```

### Notes on integrating earlier tasks

- If Task 07 already inserts `<ClientRouter />` through a shared layout/head component, use that existing head instead of adding a second router.
- If Task 09 has a shared `BaseHead.astro`, keep its production metadata conventions and add the locale-specific canonical and `hreflang` links there.
- The new pages are intentionally independent of Task 03's React-filtered `/resources` page.
- If your resource IDs are different, update the translation dictionary in Step 3.

## 6. Add the English route: `src/pages/start.astro`

```astro
---
import LocalizedStartPage from '../components/LocalizedStartPage.astro';
---

<LocalizedStartPage locale="en" />
```

## 7. Add the Russian route: `src/pages/ru/start.astro`

```astro
---
import LocalizedStartPage from '../../components/LocalizedStartPage.astro';
---

<LocalizedStartPage locale="ru" />
```

## 8. Build and verify

```bash
npm run dev
```

Visit:

```text
http://localhost:4321/start
http://localhost:4321/ru/start
```

Then:

```bash
npm run build
npm run preview
```

Inspect generated HTML and verify:

- `<html lang="en">` and `<html lang="ru">`.
- Each page has its own absolute canonical.
- Both pages advertise the two real Start-page `hreflang` alternatives.
- English article links still point to `/resources/<id>`.
- Keyboard Tab + Enter can switch language.
- The switcher still works with JavaScript disabled.
- Missing Russian summaries show the fallback notice.
- Existing `/resources` functionality still works.
- At 320px wide, no horizontal scrollbar appears because of long text.

## Alternative solution

For a larger multilingual site, store localized resource entries in a CMS or in locale-specific Content Collections and join translations using a stable shared ID. This enables **fully translated articles** and locale-preserving detail links. Keep the same Astro i18n URL helpers, but generate `hreflang` only for article translations that actually exist.

## Stretch goal suggestion

Add a real Russian translation for **one entire resource article**, generate its `/ru/resources/<slug>` route, and update the card link **only for that article**. This tests the difference between translating navigation UI and localizing the content model.

## Documentation

- https://docs.astro.build/en/guides/internationalization/
- https://docs.astro.build/en/reference/modules/astro-i18n/
