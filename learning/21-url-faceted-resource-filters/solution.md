# Task 21 — Reference solution

Try the task first. This is a **reference implementation**, not a verified build; adapt collection IDs, field names and paths to the evolving Astro project.

## Implementation

```astro
// src/lib/filterResources.ts
export type Resource = { id:string; title:string; topic:string; difficulty:string; url:string };
export function filterResources(items:Resource[], params:URLSearchParams) {
  const topics=params.getAll('topic');
  const difficulty=params.get('difficulty');
  return items.filter(item =>
    (!topics.length || topics.includes(item.topic)) &&
    (!difficulty || item.difficulty===difficulty)
  ).sort((a,b)=>a.title.localeCompare(b.title));
}

// src/components/ResourceFilters.astro
---
import type { Resource } from '../lib/filterResources';
const { resources } = Astro.props as { resources: Resource[] };
const topics = [...new Set(resources.map(r=>r.topic))].sort();
---
<section data-filters>
  <form method="get" action="/resources/">
    <fieldset><legend>Topics</legend>{topics.map(topic=>
      <label><input type="checkbox" name="topic" value={topic}/>{topic}</label>
    )}</fieldset>
    <label>Difficulty <select name="difficulty">
      <option value="">Any</option><option value="beginner">Beginner</option>
      <option value="intermediate">Intermediate</option>
    </select></label>
    <button>Apply</button><a href="/resources/">Clear</a>
  </form>
  <p data-count role="status" aria-live="polite"></p>
  <ul data-results>{resources.map(r=><li><a href={r.url}>{r.title}</a></li>)}</ul>
  <script type="application/json" data-items set:html={JSON.stringify(resources).replace(/</g,'\\u003c')} />
</section>
<script>
import { filterResources } from '../lib/filterResources';
function setup(){
 document.querySelectorAll('[data-filters]').forEach(root=>{
  if(root.dataset.ready==='yes') return;
  root.dataset.ready='yes';
  const form=root.querySelector('form'), list=root.querySelector('[data-results]');
  const count=root.querySelector('[data-count]');
  const items=JSON.parse(root.querySelector('[data-items]').textContent);
  const sync=()=>{
   const p=new URLSearchParams(location.search);
   form.querySelectorAll('[name="topic"]').forEach(el=>el.checked=p.getAll('topic').includes(el.value));
   form.elements.difficulty.value=p.get('difficulty')??'';
   const found=filterResources(items,p);
   list.replaceChildren(...found.map(r=>{
    const li=document.createElement('li'), a=document.createElement('a');
    a.href=r.url;a.textContent=r.title;li.append(a);return li;
   }));
   count.textContent=found.length+' resources found';
  };
  form.addEventListener('submit',e=>{
   e.preventDefault();
   const p=new URLSearchParams(new FormData(form));
   history.pushState(null,'',location.pathname+(p.size?'?'+p:''));
   sync();
  });
  window.addEventListener('popstate',sync);
  document.addEventListener('astro:before-swap',()=>window.removeEventListener('popstate',sync),{once:true});
  sync();
 });
}
document.addEventListener('astro:page-load',setup);
setup();
</script>
<style>
[data-results]{display:grid;grid-template-columns:repeat(auto-fit,minmax(min(100%,16rem),1fr));gap:1rem}
[data-results] li{display:grid;grid-template-rows:subgrid;grid-row:span 2}
fieldset:has(input:checked){outline:2px solid currentColor}
@media(prefers-reduced-motion:reduce){*{scroll-behavior:auto!important}}
</style>
```

## Integration notes
- Keep the existing Astro layout and collection configuration; add or adapt only the relevant component and helper.
- Verify the existing routes and the new feature in `npm run dev`.
- Run `npm run build` and `npm run preview`.
- Test keyboard navigation, reduced motion, malformed data and a 320px viewport.
- Use browser devtools to confirm no unnecessary JavaScript was shipped.

## Alternative
For a very large dataset, use an on-demand server-rendered GET page rather than serializing every resource to the client.

## Stretch goals
- Add removable selected-filter chips.
- Add URL refresh and browser-back Playwright tests.
