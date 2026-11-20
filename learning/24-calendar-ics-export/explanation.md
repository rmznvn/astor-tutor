# Task 24 — Explanation and interview practice

**Architecture:** Calendar output is a file representation. Astro's static endpoint can produce it at build time; browser hydration is unnecessary. Build small pure helpers for escaping, UTC conversion and line folding. CRLF and octet-aware folding matter for interoperability.

### Interview: Why an endpoint?
**Candidate:** “The feature is a downloadable file, not interactive UI. A GET endpoint returns a standards-based representation without shipping a JS bundle.”

### Interview: Why escape content?
**Candidate:** “iCalendar TEXT values reserve backslash, semicolon, comma and line breaks. I encode them before serializing VEVENT lines and verify Unicode.”

### Interview: Static or dynamic?
**Candidate:** “Static works when sessions change on deployment. For continuously updated schedules I would choose on-demand rendering and a cache policy.”

## Alternative
Generate ICS on the client with Blob URLs. It can work offline but increases JS, complicates validation and duplicates server logic. Prefer the endpoint unless offline editing is a requirement.

## Common mistakes
Using LF rather than CRLF, folding by JS string length instead of UTF-8 bytes, changing UIDs every build, omitting Content-Disposition and exposing invalid durations.
