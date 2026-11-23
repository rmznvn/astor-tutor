# Reference solution: Accessible Code Snippet Toolbar

This is a working-pattern reference, not a tested integration. Keep existing routes/config and adjust imports and content paths to your current Astro app. Complete the task independently before reading.

## `src/components/CodeSnippet.astro`

```astro
---
interface Props { language: string; label?: string }
const { language, label = 'Code example' } = Astro.props;
---
<figure class="snippet" data-code-snippet>
  <figcaption><span>{language}</span><button type="button" data-copy>Copy code</button></figcaption>
  <pre aria-label={label}><code><slot /></code></pre>
  <span class="sr-only" role="status" data-status></span>
</figure>
<style>
.snippet { position: relative; min-inline-size: 0; border: 1px solid currentColor; border-radius: .75rem; overflow: hidden; }
figcaption { display:flex; justify-content:space-between; align-items:center; gap:1rem; padding:.5rem 1rem; }
pre { overflow-x:auto; padding:1rem; margin:0; }
button:focus-visible { outline: 3px solid currentColor; outline-offset:2px; }
.sr-only { position:absolute; width:1px; height:1px; overflow:hidden; clip-path:inset(50%); }
@supports (anchor-name: --copy) {
  figcaption { anchor-name: --copy; }
}
@media (prefers-reduced-motion:reduce) { *, *::before { scroll-behavior:auto !important; } }
</style>
<script>
  // Event delegation survives ClientRouter page swaps without duplicate listeners.
  document.addEventListener('click', async (event) => {
    const button = (event.target as Element).closest<HTMLButtonElement>('[data-code-snippet] [data-copy]');
    if (!button) return;
    const figure = button.closest<HTMLElement>('[data-code-snippet]');
    const text = figure?.querySelector('code')?.textContent ?? '';
    const status = figure?.querySelector<HTMLElement>('[data-status]');
    try {
      if (!navigator.clipboard?.writeText) throw new Error('Clipboard API unavailable');
      await navigator.clipboard.writeText(text);
      if (status) status.textContent = 'Code copied to clipboard';
      button.textContent = 'Copied';
    } catch {
      if (status) status.textContent = 'Copy failed. Select the code manually.';
    }
  });
</script>
```

## Integration
Render `<CodeSnippet language="js">const answer = 42;</CodeSnippet>` on an existing resource article. Test Copy on HTTPS and with clipboard access denied.

## Verification
- Run `npm run build` and `npm run preview`.
- Inspect page source, network requests and browser console.
- Verify the keyboard/focus state and the error case.
- Do not merge the implementation into `src/` until you have tried the exercise.

## Alternative
A React island can offer editing/highlighting, at the cost of more hydration.
