# Task 19 — Explanation

## Core idea
Add a small automated test suite for pure search logic and critical end-to-end flows from the existing Astro project.

The important skill is not only using an API, but choosing the correct rendering and data boundary. Astro lets static HTML, request-time server rendering, and small browser interactions coexist; use the least dynamic option that satisfies the product requirement.

## Interview questions
### What should be unit-tested versus end-to-end tested?
**Strong answer:** Choose the rendering boundary based on freshness, personalization, caching, and client interactivity. Keep stable content static and isolate only the part that truly needs dynamic behavior.

### Why extract search filtering into a pure function?
**Strong answer:** Performance features are trade-offs. I verify the actual payload/cache behavior and avoid optimizations that make content stale, inaccessible, or harder to debug.

### What makes an E2E test flaky, and how do you reduce flakiness?
**Strong answer:** Tests should protect user-visible behavior and pure business logic while avoiding implementation details. Deterministic inputs and explicit waits for user-observable states reduce flaky tests.

## Common mistakes
- Making the whole page dynamic for one small dynamic area.
- Caching personalized output as public content.
- Testing internal implementation instead of observable behavior.
- Forgetting fallback, empty, error, or reduced-motion states.

## Alternative approach
You can often solve the same requirement with a client fetch or a fully server-rendered page. Compare cacheability, first render, JS cost, hosting constraints, and failure modes before choosing.
