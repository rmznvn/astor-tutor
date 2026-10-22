# Task 05 — Explanation

## Why slots matter

Props are best for data-like values:

~~~text
title
variant
id
~~~

Slots are best for chunks of markup:

~~~text
buttons
badges
cards
custom footer content
~~~

If you try to pass large chunks of HTML through props, the component API becomes awkward.

## Default vs named slot

Default slot:

~~~astro
<ContentSection>
  <Card />
  <Card />
</ContentSection>
~~~

Named slot:

~~~astro
<a slot="actions" href="/resources">View all</a>
~~~

Inside the component:

~~~astro
<slot />
<slot name="actions" />
~~~

## Why check whether a slot exists?

Optional wrappers should not appear when there is no content. `Astro.slots.has()` lets you avoid empty layout elements.

## Interview questions

### Props or slots: how do you choose?

A strong answer:

Use props for structured values and configuration, and slots when the parent needs to provide arbitrary child markup or component composition.

### Why not create three separate section components?

A strong answer:

If the structure and behavior are truly the same, one well-designed component reduces duplication. If the sections later diverge meaningfully, separate components may become clearer.

### What is the risk of an overly generic component?

A strong answer:

It can accumulate too many props and conditionals until its API becomes harder to understand than duplicated simple markup.
