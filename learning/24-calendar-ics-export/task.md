# Task 24 — Downloadable Event Calendar Feed

## Scenario
The resource hub advertises workshops. Visitors need a calendar download without browser JavaScript.

## Requirements
1. Define typed sessions with title, description, stable ID, start/end UTC ISO strings, and URL.
2. Create `src/pages/events.ics.ts` using an Astro GET endpoint.
3. Output `text/calendar`, CRLF line endings, escaped iCalendar text and UTF-8-aware 75-octet line folding.
4. Exclude invalid dates and end-before-start events; handle zero sessions.
5. Add a semantic event grid and download link on the existing events page.
6. Use CSS Grid `repeat(auto-fit,minmax())`, logical properties, focus-visible and reduced-motion styles.

## Acceptance criteria
- [ ] Calendar imports in a calendar app.
- [ ] Unicode titles, commas, semicolons and newlines survive export.
- [ ] Download works with JavaScript disabled.
- [ ] 320px layout has no horizontal overflow.
- [ ] Existing pages work and `npm run build` passes.

## Interview hints
What is the difference between an Astro endpoint and a hydrated island? How do you encode a VEVENT? Why are stable UIDs important? Try implementing escaping and folding separately before reading the solution.

## Stretch goals
Support multiple calendars, verify exported bytes in a unit test, and discuss on-demand rendering versus static export.
