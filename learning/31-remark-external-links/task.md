# Task 31 — Markdown External Link Policy

## Scenario
Add a build-time remark plugin that annotates external Markdown links, with a CSS-only visual indicator and accessible link text.

## Requirements
Write a local remark plugin using a recursive AST walk. Classify only HTTP(S) links outside the configured site origin; ignore relative, fragment and mailto links. Add a data-external attribute and visible text suffix to external links without forcing new tabs. Register the plugin in existing Astro config. Demonstrate internal and external links in an existing Markdown resource or a new fixture.

## Acceptance criteria
- [ ] External HTTPS and HTTP links get exactly one suffix
- [ ] same-origin and relative links do not
- [ ] existing link text remains
- [ ] no target=_blank added
- [ ] keyboard focus visible
- [ ] no client script.
- [ ] npm run build succeeds and previous routes still work.

## Hints (before coding)
Inspect a Markdown link node and its children. How can URL parsing distinguish origin from hostname? Where can remark attach properties for the HTML renderer?

## Stretch goals
Add tests for nested emphasis, malformed URLs and protocol-relative links; make the origin configurable for deployment previews.
