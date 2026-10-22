# Task 04 — Explanation

## Why this matters

A common mistake in content-heavy sites is to put every image in `public/` and render it with a normal `<img>`.

That works, but Astro cannot optimize files in `public/` the same way it can optimize imported assets under `src/`.

With `astro:assets`, Astro can generate appropriate output dimensions and responsive sources.

## Mental model

~~~text
local source image
      ↓
Astro Image component
      ↓
build-time image processing
      ↓
optimized output + responsive srcset
      ↓
browser chooses suitable resource
~~~

## Why a collection schema?

Without a schema, a typo such as a missing cover may only become obvious later in rendering.

A schema moves that error earlier:

~~~text
content authoring
      ↓
schema validation
      ↓
type-safe component data
~~~

## Priority vs lazy loading

Do not mark every image as high priority.

Priority is useful for an important above-the-fold image, such as a hero or featured resource. Other images should normally be allowed to load later.

## Interview questions

### Why prefer Astro Image for local content images?

A strong answer:

It lets Astro optimize and transform source assets, produce responsive image markup, and reduce unnecessary bytes while preserving intrinsic dimensions.

### Why are width and height important?

A strong answer:

They give the browser intrinsic dimensions before the file finishes loading, helping reserve space and reduce layout shift.

### Why should alt text come from content rather than the filename?

A strong answer:

Alternative text should describe the image's meaning in context. A filename is implementation detail and often useless to screen-reader users.
