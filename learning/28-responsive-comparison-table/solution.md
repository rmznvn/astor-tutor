# Reference solution: Responsive Feature Comparison from Content

This is a working-pattern reference, not a tested integration. Keep existing routes/config and adjust imports and content paths to your current Astro app. Complete the task independently before reading.

## `src/lib/comparison.ts`

```ts
export interface Feature {
  id: string;
  label: string;
  values: Record<string, string>;
}
export interface Product { id: string; label: string }
export function normalizeComparison(products: Product[], features: Feature[]) {
  const ids = new Set(products.map(p => p.id));
  if (ids.size !== products.length) throw new Error('Duplicate product id');
  const featureIds = new Set<string>();
  for (const feature of features) {
    if (featureIds.has(feature.id)) throw new Error('Duplicate feature id');
    featureIds.add(feature.id);
    for (const key of Object.keys(feature.values)) {
      if (!ids.has(key)) throw new Error(`Unknown product: ${key}`);
    }
  }
  return features.map(feature => ({
    ...feature,
    cells: products.map(product => feature.values[product.id] ?? 'Not specified')
  }));
}
```

## `src/components/ComparisonTable.astro`

```astro
---
import { normalizeComparison, type Product, type Feature } from '../lib/comparison';
interface Props { products: Product[]; features: Feature[]; caption: string }
const { products, features, caption } = Astro.props;
const rows = normalizeComparison(products, features);
---
<div class="comparison-scroll" tabindex="0" role="region" aria-label={caption}>
  <table>
    <caption>{caption}</caption>
    <thead><tr><th scope="col">Feature</th>{products.map(p => <th scope="col">{p.label}</th>)}</tr></thead>
    <tbody>{rows.map(row => <tr><th scope="row">{row.label}</th>{row.cells.map(value => <td>{value}</td>)}</tr>)}</tbody>
  </table>
</div>
<style>
.comparison-scroll { overflow-x:auto; max-inline-size:100%; scrollbar-gutter:stable; }
table { border-collapse:separate; border-spacing:0; min-inline-size:42rem; inline-size:100%; }
th,td { padding:.75rem; border-block-end:1px solid #7776; text-align:start; }
tbody th { position:sticky; inset-inline-start:0; background:Canvas; z-index:1; }
.comparison-scroll:focus-visible { outline:3px solid currentColor; outline-offset:2px; }
@supports (grid-template-columns:subgrid) {
  /* Optional card-layout experiment: preserve the semantic table as the default. */
  .comparison-cards { grid-template-columns:subgrid; }
}
</style>
```

## Integration
Pass `products` and `features` arrays to `<ComparisonTable products={products} features={features} caption="Technology comparison" />` on an existing resources page. Adapt the arrays from your collection data.

## Verification
- Run `npm run build` and `npm run preview`.
- Inspect page source, network requests and browser console.
- Verify the keyboard/focus state and the error case.
- Do not merge the implementation into `src/` until you have tried the exercise.

## Alternative
A card grid can be easier on phones, but duplicates header labels and needs careful accessibility.
