# Task 10 — Bilingual landing page with Astro i18n

**Level:** Intermediate · **Suggested time:** 2–4 hours  
**Scenario:** A marketing/education website needs an English and Russian entry page. Existing resource articles are currently written in English. The business wants a translated landing page now, without pretending the full article library is translated.

## Goal

Build two real, crawlable, statically generated pages from **one shared Astro component**:

- `/start` — English
- `/ru/start` — Russian

Both pages should display a localized introduction, a keyboard-accessible language switcher, and up to three cards drawn from your existing `resources` Content Collection.

Keep existing routes, including `/resources` and `/resources/[slug]`, intact.

## Astro skills

- Built-in `i18n` configuration: `locales`, `defaultLocale`, `prefixDefaultLocale`
- Localized file-based routing in `src/pages/ru/`
- `getRelativeLocaleUrl()` from `astro:i18n`
- Shared page components and typed props
- `getCollection('resources')` reused across locales
- A typed UI dictionary and partial content translations
- Correct `<html lang>`, canonical, and `hreflang` links
- Progressive enhancement without client-side translation libraries

## Required structure

```text
src/
├── components/
│   ├── LanguageSwitcher.astro
│   └── LocalizedStartPage.astro
├── i18n/
│   ├── ui.ts
│   └── resourceSummaries.ts
└── pages/
    ├── start.astro
    └── ru/
        └── start.astro
```

Update `astro.config.mjs` **without deleting existing integrations, site configuration, or other settings**.

## Functional requirements

1. Set `en` as the default locale without an `/en` prefix; enable `ru` as a prefixed locale.
2. Both routes render the same page component with a different locale prop.
3. Translate the title, intro, navigation labels, CTA, empty state, and language-switcher accessible label.
4. Provide Russian summaries for at least two known resource IDs. Fall back to English text for resources without a Russian summary, **with an honest visible notice**.
5. Link resource cards to their existing English article routes. Label these links as English-only content; **do not create fake `/ru/resources/[slug]` links**.
6. The language switcher navigates to the corresponding version of the **same Start page**, not just the homepage.
7. Render `lang="en"` or `lang="ru"` on the root HTML element.
8. Include an absolute canonical URL and reciprocal `rel="alternate" hreflang` links on both pages.
9. Preserve Task 07's `ClientRouter` setup if you already use it.
10. Avoid automatic locale redirects, localStorage, and unnecessary client JavaScript.

## Modern CSS practice (different emphasis from previous tasks)

Practice **language-aware typography** using `:lang(ru)`, `hyphens: auto`, `text-wrap: pretty`, `text-wrap: balance`, logical spacing properties, and a responsive CSS Grid. Add a subtle animated underline for the language switcher and a restrained card-hover transform. Respect `prefers-reduced-motion`.

## Accessibility and performance

- A real `<nav aria-label>` contains normal `<a href>` links.
- The active locale has `aria-current="page"`.
- Both languages have a visible keyboard focus state.
- Language names should use their own `lang` attributes (e.g. `lang="ru"` for “Русский”).
- All meaningful content is in generated HTML; no browser-side translation or hydration is required.
- Avoid visual-only cues: translation fallbacks and English-only articles must be stated in text.
- Test long Russian words at 320px viewport width.

## Acceptance criteria

- [ ] `npm run build` succeeds.
- [ ] `/start` and `/ru/start` both exist in the static output.
- [ ] English URLs remain unprefixed; Russian URL has `/ru/`.
- [ ] Both pages reuse one component and one collection.
- [ ] Language switcher works with keyboard, mouse, and JavaScript disabled.
- [ ] `html[lang]`, canonical, and `hreflang` are correct on both pages.
- [ ] The English-only article link is clearly labeled in both languages.
- [ ] Missing Russian summaries show a visible fallback notice.
- [ ] The layout remains readable at 320px, 768px, and desktop widths.
- [ ] No existing resources routes or filters are removed.

## Hints (try before opening the solution)

1. Configure i18n first, then add the `src/pages/ru/` folder.
2. Use two **thin route files** that pass different props to one shared page component.
3. `getRelativeLocaleUrl(locale, 'start')` knows whether the default locale needs a prefix.
4. Keep UI translations separate from Content Collection data.
5. Store partial translations keyed by `entry.id`, with a deliberate English fallback.
6. Do not assume that translating a title also translates the Markdown article body.
7. An absolute canonical URL can be built with `new URL(relativePath, Astro.site ?? Astro.url)`.
8. For `hreflang`, only advertise real translated equivalents of the same page.

## Stretch goals

- Add a third locale (e.g. German) without duplicating the shared page component.
- Move the summary translations into locale-specific content entries with stable translation IDs.
- Localize resource detail pages **only after** translating their full content, then preserve article slugs in the language switcher.
- Format dates and numbers with `Intl.DateTimeFormat` / `Intl.NumberFormat`.
- Add a small locale suggestion banner based on browser preferences **without forcibly redirecting users**.

## Reference docs

- https://docs.astro.build/en/guides/internationalization/
- https://docs.astro.build/en/reference/modules/astro-i18n/
