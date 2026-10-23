# Task 10 — Explanation and interview preparation

## 1. URL routing is not the same as translation

Astro's built-in i18n support helps define and generate language-aware URLs. It does **not** translate text automatically.

```text
astro.config.mjs
  i18n: en (default), ru
         |
         +-- /start       -> src/pages/start.astro
         +-- /ru/start    -> src/pages/ru/start.astro

Both route files
         |
         v
LocalizedStartPage.astro
         |
         +-- ui.ts                (labels, headings)
         +-- resourceSummaries.ts (partial translated summaries)
         +-- getCollection()      (real resource data)
```

This is useful for product sites where you can launch a localized marketing page before every article has been translated.

## 2. Why use `prefixDefaultLocale: false`?

The existing project already uses unprefixed English routes such as `/resources`. With English as the default locale and `prefixDefaultLocale: false`, you can add `/ru/start` without renaming existing routes or breaking incoming links.

Astro's file-based routes still need matching files under `src/pages/` and `src/pages/ru/`. Configuring i18n alone does not generate translated pages.

## 3. Why use `getRelativeLocaleUrl()`?

Avoid manual strings such as `'/' + locale + '/start'`. They often produce incorrect URLs for the default locale or after configuration changes.

`getRelativeLocaleUrl('en', 'start')` returns the unprefixed English route, while `getRelativeLocaleUrl('ru', 'start')` returns the Russian route under this configuration.

## 4. Why separate UI copy from resource summaries?

UI copy is small and predictable: “Read article”, “Switch language”, “No resources yet”.

Resource content has different editorial ownership and translation status. Keeping them separate makes missing translations visible and makes future CMS migration easier.

A partial translation dictionary should not silently make the interface claim the **whole article** is in Russian.

## 5. Canonical vs hreflang

- A **canonical** URL identifies the preferred URL for the *current page*. The English page canonicals to English, and the Russian page canonicals to Russian.
- `hreflang` points to **real alternative-language versions** of the same content. Both Start pages should list both versions.
- Do not point the Russian canonical to English just because some linked articles remain English.
- Do not advertise `/ru/resources/some-slug` unless that translated article actually exists.

Absolute URLs are preferable for canonical and alternate metadata. Use the real production `site` URL from Task 09 rather than a placeholder when deploying.

## 6. CSS and Russian typography

Text lengths vary substantially by language. A layout that works for English may overflow in Russian.

- `:lang(ru)` selects elements according to their language.
- `hyphens: auto` can improve wrapping if the browser has an appropriate hyphenation dictionary.
- `text-wrap: balance` is useful for short headings; `text-wrap: pretty` can improve paragraph wrapping.
- Logical properties such as `padding-inline` and `margin-block` avoid tying spacing to physical left/right directions.
- Never rely on hyphenation alone: also provide robust grid min sizes and overflow behavior.

## 7. Why no React island?

All the content can be generated statically. Switching language is navigation, not a stateful UI that needs hydration. Real links work even when JavaScript is unavailable.

If you already use Astro `ClientRouter`, its enhanced navigation can remain. It is not required to make the language switcher functional.

## 8. Common mistakes to avoid

1. Using `/en/start` even though `prefixDefaultLocale` is false.
2. Adding `i18n` config but forgetting the physical `ru/start.astro` route.
3. Translating headings but leaving `<html lang="en">` on the Russian page.
4. Sending users to a non-existent translated article route.
5. Marking a Russian page's canonical as English.
6. Rendering `hreflang` alternatives for pages that do not exist.
7. Replacing the existing React-filtered `/resources` page just to add a localized landing page.
8. Assuming `Astro.currentLocale` changes automatically inside arbitrary client-side components.

## Interview questions with strong answers

**Q: What does Astro i18n actually do?**  
**A:** It provides locale-aware routing conventions, configuration, and URL helpers. Translation content and the editorial process are still the application's responsibility.

**Q: Why choose file-based localized routes over manually inspecting `window.location`?**  
**A:** File-based routes generate crawlable HTML, work without JavaScript, integrate with Astro's build pipeline, and avoid client-side locale routing bugs.

**Q: How do you handle incomplete translations?**  
**A:** I define an explicit fallback policy. I display the available language, label untranslated content honestly, and only publish localized URLs when that page really exists.

**Q: What is the difference between canonical and hreflang?**  
**A:** Canonical identifies the preferred URL of a page; hreflang identifies equivalent pages in other languages or regions. They solve different indexing problems and should not contradict the actual content.

**Q: Why are locale-aware URLs better than storing language only in localStorage?**  
**A:** URLs can be bookmarked, shared, indexed, and loaded directly. LocalStorage is unavailable during static generation and should not be the sole source of locale state.

**Q: How do you prevent regressions while localizing an existing site?**  
**A:** I add new localized routes incrementally, keep current URLs stable, reuse shared components, and test navigation, metadata, focus, build output, and long-text layouts.

## Alternative architectures

For a few static languages, a typed UI dictionary is simple and has no runtime dependency. For many articles and editors, locale-specific Content Collection entries or a CMS with translation relationships is usually better. You can still use Astro's built-in i18n URL helpers with either content strategy.

## Official references

- https://docs.astro.build/en/guides/internationalization/
- https://docs.astro.build/en/reference/modules/astro-i18n/
