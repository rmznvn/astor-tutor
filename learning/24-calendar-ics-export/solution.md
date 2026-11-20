# Task 24 — Reference solution

```ts
// src/data/sessions.ts
export const sessions = [
  { id: 'astro-workshop', title: 'Astro workshop',
    description: 'Accessible UI', start: '2027-01-12T17:00:00Z',
    end: '2027-01-12T18:00:00Z', url: 'https://example.com/events/astro' },
];

// src/pages/events.ics.ts
import type { APIRoute } from 'astro';
import { sessions } from '../data/sessions';
// Escape characters reserved by RFC 5545 TEXT values.
const escapeText = (s: string) => s.replace(/\\/g, '\\\\')
  .replace(/\r\n|\n|\r/g, '\\n').replace(/,/g, '\\,').replace(/;/g, '\\;');
const utc = (s: string) => new Date(s).toISOString()
  .replace(/[-:]/g, '').replace(/\.\d{3}/, '');
// Fold by UTF-8 octets without splitting a Unicode code point.
function fold(line: string) {
  let output = '', bytes = 0;
  for (const ch of line) {
    const count = new TextEncoder().encode(ch).length;
    if (bytes + count > 75) { output += '\r\n '; bytes = 1; }
    output += ch; bytes += count;
  }
  return output;
}
export const GET: APIRoute = () => {
  const lines = ['BEGIN:VCALENDAR', 'VERSION:2.0',
    'PRODID:-//Astro Tutor//Sessions//EN', 'CALSCALE:GREGORIAN'];
  for (const e of sessions) {
    const start = Date.parse(e.start), end = Date.parse(e.end);
    if (!Number.isFinite(start) || !Number.isFinite(end) || end <= start) continue;
    lines.push('BEGIN:VEVENT', 'UID:' + e.id + '@astro-tutor.example',
      'DTSTAMP:' + utc(new Date().toISOString()),
      'DTSTART:' + utc(e.start), 'DTEND:' + utc(e.end),
      'SUMMARY:' + escapeText(e.title),
      'DESCRIPTION:' + escapeText(e.description),
      'URL:' + e.url, 'END:VEVENT');
  }
  lines.push('END:VCALENDAR');
  return new Response(lines.map(fold).join('\r\n') + '\r\n', {
    headers: { 'Content-Type': 'text/calendar; charset=utf-8',
      'Content-Disposition': 'attachment; filename="study-sessions.ics"' }
  });
};
```

Add `<a href="/events.ics" download>Download calendar</a>` to the existing events page.

```css
.events-grid { display: grid; grid-template-columns: repeat(auto-fit,minmax(min(100%,17rem),1fr)); gap: 1rem; }
.events-grid a:focus-visible { outline: 2px solid currentColor; outline-offset: .2rem; }
@media (prefers-reduced-motion: reduce) { .events-grid * { animation: none; transition: none; } }
```

**Verification:** Build, inspect generated `dist/events.ics`, import into a calendar, test Unicode/empty data. **Alternative:** On-demand endpoint with adapter for rapidly changing events.
