# Task 32 — Custom Astro Integration & Build Metrics

## Concepts and reasoning
Astro integrations register lifecycle hooks. The build:done hook receives the output directory as a URL; Node fs supports file URLs and path conversion. Measuring raw bytes is deterministic but differs from compressed transfer size. A local file input avoids publishing build diagnostics. TextContent prevents markup injection.

## Interview questions and sample answers
**Why use an integration rather than a page endpoint?** I need final output files, which do not exist until rendering finishes.

**Why report raw bytes?** It is reproducible and useful for regression detection; I would separately measure Brotli or gzip transfer sizes.

**How do you safely render imported JSON?** Validate types and bounds, then create elements and assign textContent rather than innerHTML.

## Alternative approach
A standalone postbuild Node script is easier to reuse outside Astro; an integration is preferable when you want access to Astro lifecycle data.
