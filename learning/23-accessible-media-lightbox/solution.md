# Task 23 — Reference solution

Try the task first. This is a **reference implementation**, not a verified build; adapt collection IDs, field names and paths to the evolving Astro project.

## Implementation

```astro
// src/components/Gallery.astro
---
import { Image } from 'astro:assets';
import type { ImageMetadata } from 'astro';
type Photo={src:ImageMetadata;alt:string;caption?:string};
const {photos}=Astro.props as {photos:Photo[]};
---
<section class="gallery" data-gallery aria-label="Screenshot gallery">
 <div class="strip">
  {photos.map((photo,index)=>
   <button type="button" data-index={index} aria-label={'View image '+(index+1)}>
    <Image src={photo.src} alt={photo.alt} width={240} loading="lazy"/>
   </button>
  )}
 </div>
 <dialog aria-label="Image viewer">
  <nav aria-label="Image controls">
   <button type="button" data-prev>Previous</button>
   <button type="button" data-next>Next</button>
   <button type="button" data-close>Close</button>
  </nav>
  <img data-full alt=""/><p data-caption></p>
 </dialog>
 <script type="application/json" data-photos set:html={JSON.stringify(photos.map(p=>({
  src:p.src.src,alt:p.alt,caption:p.caption??''
 }))).replace(/</g,'\\u003c')} />
</section>
<script>
function initialize(){
 document.querySelectorAll('[data-gallery]').forEach(root=>{
  if(root.dataset.initialized==='true')return;
  root.dataset.initialized='true';
  const controller=new AbortController(), signal=controller.signal;
  const photos=JSON.parse(root.querySelector('[data-photos]').textContent);
  if(!photos.length)return;
  const dialog=root.querySelector('dialog'),image=root.querySelector('[data-full]');
  const caption=root.querySelector('[data-caption]');
  let index=0,trigger;
  function show(){image.src=photos[index].src;image.alt=photos[index].alt;caption.textContent=photos[index].caption}
  function move(delta){index=(index+delta+photos.length)%photos.length;show()}
  root.querySelectorAll('[data-index]').forEach(button=>button.addEventListener('click',()=>{
   trigger=button;index=Number(button.dataset.index);show();dialog.showModal();
  },{signal}));
  dialog.querySelector('[data-prev]').addEventListener('click',()=>move(-1),{signal});
  dialog.querySelector('[data-next]').addEventListener('click',()=>move(1),{signal});
  dialog.querySelector('[data-close]').addEventListener('click',()=>dialog.close(),{signal});
  dialog.addEventListener('keydown',e=>{
   if(e.key==='ArrowLeft'){e.preventDefault();move(-1)}
   if(e.key==='ArrowRight'){e.preventDefault();move(1)}
  },{signal});
  dialog.addEventListener('close',()=>trigger?.focus(),{signal});
  document.addEventListener('astro:before-swap',()=>controller.abort(),{once:true,signal});
 });
}
document.addEventListener('astro:page-load',initialize);
initialize();
</script>
<style>
.strip{display:flex;gap:.75rem;overflow-x:auto;scroll-snap-type:x mandatory}
.strip button{flex:0 0 min(45vw,12rem);scroll-snap-align:start;padding:0;border:0}
.strip img{display:block;inline-size:100%;aspect-ratio:4/3;object-fit:cover}
dialog{max-inline-size:min(90vw,70rem);max-block-size:90dvh;border:0;border-radius:1rem}
dialog::backdrop{background:rgb(0 0 0 / .8)}
dialog img{max-inline-size:100%;max-block-size:65dvh;object-fit:contain}
button:focus-visible{outline:3px solid currentColor;outline-offset:3px}
@media(prefers-reduced-motion:reduce){.strip{scroll-behavior:auto}}
</style>
```

## Integration notes
- Keep the existing Astro layout and collection configuration; add or adapt only the relevant component and helper.
- Verify the existing routes and the new feature in `npm run dev`.
- Run `npm run build` and `npm run preview`.
- Test keyboard navigation, reduced motion, malformed data and a 320px viewport.
- Use browser devtools to confirm no unnecessary JavaScript was shipped.

## Alternative
A React island may be appropriate for gestures, zoom and annotations, but native dialog minimizes JavaScript for a simple viewer.

## Stretch goals
- Show a visible image counter.
- Add responsive full-size srcset.
- Test keyboard and focus restoration with Playwright.
