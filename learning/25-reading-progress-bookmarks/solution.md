# Task 25 — Reference implementation

```astro
<!-- src/components/ReadingProgress.astro -->
<div data-reading-progress>
  <label for="article-progress">Reading progress</label>
  <progress id="article-progress" max="100" value="0">0%</progress>
  <output for="article-progress">0%</output>
</div>
<script>
  let cleanup: (() => void) | undefined;
  function setup() {
    cleanup?.();
    const root = document.querySelector('[data-reading-progress]');
    const progress = root?.querySelector('progress');
    const output = root?.querySelector('output');
    const article = document.querySelector('article');
    if (!progress || !output || !article) return;
    let frame = 0;
    const update = () => {
      frame = 0;
      const box = article.getBoundingClientRect();
      const distance = Math.max(1, article.offsetHeight - innerHeight);
      const value = Math.max(0, Math.min(100, Math.round(-box.top / distance * 100)));
      progress.value = value;
      output.textContent = value + '%';
    };
    const schedule = () => { if (!frame) frame = requestAnimationFrame(update); };
    addEventListener('scroll', schedule, { passive: true });
    addEventListener('resize', schedule);
    update();
    cleanup = () => {
      removeEventListener('scroll', schedule);
      removeEventListener('resize', schedule);
      if (frame) cancelAnimationFrame(frame);
    };
  }
  document.addEventListener('astro:page-load', setup);
  document.addEventListener('astro:before-swap', () => cleanup?.());
  setup();
</script>
<style>
  [data-reading-progress] { display: grid; gap: .25rem; }
  progress { inline-size: 100%; accent-color: #d59a12; }
  :global(article :is(h2,h3)[id]) { scroll-margin-block-start: 6rem; }
  @media (prefers-reduced-motion: reduce) {
    :global(html) { scroll-behavior: auto; }
  }
</style>
```

Add `<ReadingProgress />` to the existing article template. Example heading: `<h2 id="overview">Overview <a href="#overview" aria-label="Link to Overview">#</a></h2>`.

**Checks:** Test top/bottom, short article, ClientRouter back/forward, keyboard and 320px. **Alternative:** CSS scroll timeline for visual progress, with native progress as semantic fallback.
