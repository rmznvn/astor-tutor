# Reference solution: Offline Reading Pack with Service Worker

This is a working-pattern reference, not a tested integration. Keep existing routes/config and adjust imports and content paths to your current Astro app. Complete the task independently before reading.

## `public/offline-sw.js`

```js
const CACHE = 'astro-reading-v1';
const MAX_PAGES = 12;
self.addEventListener('install', () => self.skipWaiting());
self.addEventListener('activate', event => {
  event.waitUntil((async () => {
    for (const name of await caches.keys()) {
      if (name.startsWith('astro-reading-') && name !== CACHE) await caches.delete(name);
    }
    await self.clients.claim();
  })());
});
self.addEventListener('message', event => {
  if (event.data?.type !== 'SAVE_ARTICLE') return;
  const port = event.ports?.[0];
  event.waitUntil((async () => {
    try {
      const url = new URL(event.data.url, self.location.origin);
      if (url.origin !== self.location.origin || !url.pathname.startsWith('/resources/')) throw new Error('Unsupported URL');
      const response = await fetch(url.href, { credentials: 'omit' });
      if (!response.ok || !response.headers.get('content-type')?.includes('text/html')) throw new Error('Not an HTML page');
      const cache = await caches.open(CACHE);
      await cache.put(url.href, response);
      const keys = await cache.keys();
      while (keys.length > MAX_PAGES) await cache.delete(keys.shift());
      port?.postMessage({ ok:true });
    } catch (error) { port?.postMessage({ ok:false, message:String(error) }); }
  })());
});
self.addEventListener('fetch', event => {
  if (event.request.method !== 'GET' || event.request.mode !== 'navigate') return;
  const url = new URL(event.request.url);
  if (url.origin !== self.location.origin || !url.pathname.startsWith('/resources/')) return;
  event.respondWith((async () => {
    try { return await fetch(event.request); }
    catch { return (await caches.match(event.request)) ?? Response.error(); }
  })());
});
```

## `src/components/SaveOffline.astro`

```astro
---
const { url } = Astro.props;
---
<button type="button" data-offline-url={url}>Save for offline</button>
<span role="status" data-offline-status></span>
<script>
  if ('serviceWorker' in navigator && location.protocol === 'https:') {
    navigator.serviceWorker.register('/offline-sw.js').catch(console.error);
  }
  document.addEventListener('click', async (event) => {
    const button = (event.target as Element).closest<HTMLButtonElement>('[data-offline-url]');
    if (!button) return;
    const status = button.nextElementSibling;
    try {
      const registration = await navigator.serviceWorker.ready;
      const worker = registration.active;
      if (!worker) throw new Error('Offline support not ready');
      const channel = new MessageChannel();
      const result = new Promise<any>((resolve, reject) => {
        const timeout = setTimeout(() => reject(new Error('Timed out')), 8000);
        channel.port1.onmessage = ({ data }) => { clearTimeout(timeout); resolve(data); };
      });
      worker.postMessage({ type:'SAVE_ARTICLE', url:button.dataset.offlineUrl }, [channel.port2]);
      const outcome = await result;
      if (!outcome.ok) throw new Error(outcome.message);
      if (status) status.textContent = 'Saved for offline reading';
    } catch {
      if (status) status.textContent = 'Could not save offline';
    }
  });
</script>
```

## Integration
Render `<SaveOffline url={Astro.url.pathname} />` on a public resource page. Confirm the existing route prefix matches `/resources/`; update both the component and worker if your project uses another path. Service-worker HTML caching alone does not guarantee offline CSS/images/JS.

## Verification
- Run `npm run build` and `npm run preview`.
- Inspect page source, network requests and browser console.
- Verify the keyboard/focus state and the error case.
- Do not merge the implementation into `src/` until you have tried the exercise.

## Alternative
A Workbox-based service worker is better for production precaching and complex runtime caching.
