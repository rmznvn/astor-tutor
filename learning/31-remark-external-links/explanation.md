# Task 31 — Markdown External Link Policy

## Concepts and reasoning
Remark transforms the Markdown AST before HTML generation. The data.hProperties field carries attributes through mdast-to-hast. A recursive walk avoids a new dependency for this small plugin. Origin includes scheme and port, so a same-host different-port URL is external. A real text suffix is available to assistive technology.

## Interview questions and sample answers
**Why use a build-time plugin?** I can apply a consistent editorial policy to every Markdown page without shipping DOM-scanning JavaScript.

**Why not use startsWith on the URL?** A lookalike hostname can share a prefix; URL.origin compares parsed origins.

**Why not use a CSS icon alone?** Generated content is not a dependable accessible explanation; actual link text communicates the distinction.

## Alternative approach
Use a rehype plugin if you need to transform generated HTML links as well as Markdown link nodes.
