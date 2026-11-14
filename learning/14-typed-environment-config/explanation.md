# Task 14 — Explanation

## Core idea
Introduce type-safe public/server environment variables and build a reusable environment/status component without leaking server secrets.

This task is deliberately cumulative: integrate the feature into the existing layout/content architecture rather than rebuilding the app around it.

## Architecture
Think in boundaries: build/server work belongs in Astro server code; browser interaction should receive only the data and JavaScript it truly needs. Keep progressive enhancement as the default.

## Interview questions
### Why are public environment variables not secret?
**Strong answer:** Use the Astro primitive when the concern belongs to request/build rendering, because it keeps routing and server behavior centralized while preserving static-first pages.

### What benefit does a typed env schema give over raw `import.meta.env`?
**Strong answer:** Client-side validation improves UX, but server-side validation is authoritative because requests can bypass the browser UI. Validate and constrain data again at the server boundary.

### Where should secrets be consumed in an Astro app?
**Strong answer:** Keep credentials and privileged operations in server-only code. Pass only the minimal safe result to the browser and avoid serializing secrets into HTML or client bundles.

## Common mistakes
- Moving server-only data into client props.
- Adding hydration for purely presentational UI.
- Forgetting cleanup or fallback behavior.
- Treating a successful dev render as proof that build/preview behavior is correct.

## Alternative approach
A more custom endpoint/framework solution is possible, but prefer the Astro-native feature here so you learn its lifecycle and trade-offs first. Choose the alternative when requirements exceed what the native primitive expresses cleanly.
