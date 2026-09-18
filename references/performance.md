# Performance

## Targets
- Lighthouse performance **≥ 90 on mobile** (throttled, production build) — mobile is the hard one, desktop is usually fine.
- Core Web Vitals: **LCP ≤ 2.5 s, CLS ≤ 0.1, INP ≤ 200 ms** (in the lab, Total Blocking Time is the stand-in for INP).

## Measure
- Lighthouse CLI on the production build, mobile + desktop, 3 runs per page, use the median.
- Also run PageSpeed Insights on the deployed URL (real hosting, CDN, compression).
- Bundle analysis: Next → `@next/bundle-analyzer`; Angular → `ng build --stats-json` + `source-map-explorer`, and check the `budgets` in `angular.json`; Vite → `rollup-plugin-visualizer`.
- Browser console and network tab: no errors, no 404s, no duplicate requests.

## Essential

### Images (usually the biggest win)
- Modern formats (AVIF/WebP), sized for their display size (`srcset`/`sizes`), compressed.
- `width` and `height` (or `aspect-ratio`) set on every image → no layout shift.
- Below-the-fold images `loading="lazy"`. The **LCP image is never lazy** and gets `fetchpriority="high"`.
- Use the framework tools: Next `next/image` (with `priority` on the hero), Angular `NgOptimizedImage` (with `priority`).
- Screenshots/GIFs: GIFs → `<video autoplay muted loop playsinline>` in MP4/WebM, with a `poster`.

### Fonts
- Self-hosted (also required for privacy in Germany, see legal), `woff2`, only the weights you use, `font-display: swap`.
- Preload only the one main font. Next: `next/font` does all of this.

### JavaScript & CSS
- Remove unused dependencies and code. Replace heavy libraries used for small tasks (e.g. `moment` → `Intl`/`date-fns`, full `lodash` → native).
- Code-split: lazy-load routes and heavy components (dynamic `import()`, Angular lazy routes / `@defer`, `next/dynamic`).
- Next.js: Server Components by default, `'use client'` only where interaction is needed; static rendering where possible.
- Third-party scripts only if really needed, loaded late (`defer`, `next/script` with `lazyOnload`).
- No render-blocking CSS from unused frameworks/icon sets (import single icons, not the whole library).

### Layout stability & loading
- Reserve space for things that load later (images, embeds, ads, dynamic content) → CLS near 0.
- Loading states that match the final layout (skeletons) instead of content jumping in.
- Long lists: pagination or virtualization.

### Delivery
- Hashed static assets with long cache headers, text compressed (gzip/brotli). Vercel/Netlify do this by default — verify, don't assume.
- `preconnect` to origins needed early (e.g. the Supabase URL).

## Extra mile
- Lighthouse CI — see `code-quality.md` Extra mile (one setup, covers performance together with accessibility/best practices/SEO).
- Angular `budgets` / `size-limit` to fail the build when bundles grow.
- Prefetching of likely next routes.
- Optimistic UI for common actions.

## Report
Table: page × device → performance score, LCP, CLS, TBT, before/after. Biggest bundles before/after. Fixes in one sentence each.
