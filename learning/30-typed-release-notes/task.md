# Task 30 — Typed Release Notes & Native Disclosure

## Scenario
Build a release history for the knowledge hub from a dedicated Astro content collection, with expandable details and CSS grid timeline.

## Requirements
Create a glob-loaded releases collection with validated dates and categories. Sort newest first with a deterministic tie-breaker. Render a static /releases route with native disclosure, visible category labels and an empty state. Use a CSS grid timeline with logical borders and no client JavaScript.

## Acceptance criteria
- [ ] Two sample releases appear newest first
- [ ] keyboard toggles details
- [ ] invalid category fails validation
- [ ] empty collection shows a message
- [ ] no new browser JS
- [ ] no horizontal overflow at 320px.
- [ ] npm run build succeeds and previous routes still work.

## Hints (before coding)
Which information belongs in the schema? Can the browser handle disclosure without a framework? How can a grid reserve a narrow timeline column?

## Stretch goals
Add year archives using getStaticPaths; add a release RSS feed.
