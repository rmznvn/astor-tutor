# Task 26 — Explanation and interview answers

**Architecture:** Astro's content collection APIs run in its build environment, not a plain Node process. A static endpoint can produce an audit artifact at build time; a small Node checker then evaluates it. The pure validation function stays independent and easy to test.

### Interview: Why audit beyond schema validation?
**Candidate:** “A schema validates each entry's shape. A collection-wide audit can detect duplicate routes and broken relationships between entries.”

### Interview: Why separate pure logic?
**Candidate:** “It makes the rules deterministic and testable without Astro runtime. The endpoint only adapts content entries to a normalized structure.”

### Interview: Which findings should fail?
**Candidate:** “Broken references and duplicate slugs can break navigation, so they are errors. Missing descriptions may be editorial warnings. I keep severity explicit.”

## Common mistakes
Importing `astro:content` directly into a Node CLI; assuming a server-rendered endpoint creates a file in `dist`; confusing optional decorative images with missing alt text; hard-coding field names without adapting the collection schema.

## Alternative
Use an Astro integration build hook to write the report, or query the CMS API in CI. The static endpoint is simpler for a static project, while a hook is preferable for SSR output.

## CSS practice
Use `@container` for compact/wide checklist cards and `:has(input:checked)` to style completed checks. Preserve visible keyboard focus and avoid color-only status cues.
