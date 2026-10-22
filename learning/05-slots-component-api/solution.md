# Task 05 — Reference Solution

## `src/components/ContentSection.astro`

~~~astro
---
type Props = {
  id?: string;
  title: string;
  description?: string;
  variant?: 'default' | 'highlighted';
};

const {
  id,
  title,
  description,
  variant = 'default',
} = Astro.props;

const hasEyebrow = Astro.slots.has('eyebrow');
const hasActions = Astro.slots.has('actions');
const hasFooter = Astro.slots.has('footer');
---

<section
  id={id}
  class:list={['content-section', `content-section--${variant}`]}
>
  <header class="content-section__header">
    <div class="content-section__heading">
      {hasEyebrow && (
        <div class="content-section__eyebrow">
          <slot name="eyebrow" />
        </div>
      )}

      <h2>{title}</h2>

      {description && <p>{description}</p>}
    </div>

    {hasActions && (
      <div class="content-section__actions">
        <slot name="actions" />
      </div>
    )}
  </header>

  <div class="content-section__content">
    <slot />
  </div>

  {hasFooter && (
    <footer class="content-section__footer">
      <slot name="footer" />
    </footer>
  )}
</section>
~~~

## Usage

~~~astro
<ContentSection
  id="latest"
  title="Latest resources"
  description="Recent practical notes and exercises."
>
  <span slot="eyebrow">Learn</span>
  <a slot="actions" href="/resources">View all →</a>

  <ResourcesGrid resources={resources} />

  <details slot="footer" class="section-details">
    <summary>How are resources selected?</summary>
    <p>Featured and recently updated resources appear first.</p>
  </details>
</ContentSection>
~~~

## CSS

~~~css
.content-section {
  --section-gap: clamp(1.5rem, 4vw, 3rem);

  padding-block: clamp(3rem, 8vw, 7rem);
  padding-inline: max(1rem, env(safe-area-inset-left));
}

.content-section__header {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  gap: var(--section-gap);
  align-items: end;
  margin-block-end: var(--section-gap);
}

.content-section__heading {
  display: grid;
  gap: .65rem;
}

.content-section__content {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  gap: 1rem;
}

.content-section__content > :where(*) {
  grid-column: 1 / -1;
}

.content-section:has(.section-details[open]) {
  --section-gap: clamp(2rem, 5vw, 3.5rem);
}

.section-details {
  border-block-start: 1px solid rgb(255 255 255 / .12);
  padding-block-start: 1rem;
}

.section-details summary {
  cursor: pointer;
}

@media (max-width: 40rem) {
  .content-section__header {
    grid-template-columns: 1fr;
    align-items: start;
  }
}
~~~

## Why this API works

It keeps data/configuration in props while allowing rich parent-owned markup through slots. Optional slot wrappers only exist when necessary.
