# SEO & Link Previews

For portfolio projects, the most visible part of SEO is the **link preview**: recruiters get your links via LinkedIn, Slack or email. A broken or empty preview looks unfinished.

## Measure
- Lighthouse SEO category (runs together with accessibility).
- Check the rendered HTML **without JavaScript** (`curl <url>` or "view source"): are title, description and Open Graph tags in there?
- Preview check with LinkedIn Post Inspector and opengraph.xyz (after deploy).

## Essential
- Every page: unique `<title>` (~50–60 characters) and `<meta name="description">` (~150–160 characters). Format e.g. `Page – Project Name`.
- `<html lang>`, `<meta charset="utf-8">`, viewport meta tag.
- **Open Graph + Twitter Card:** `og:title`, `og:description`, `og:image` (1200×630 px, absolute URL), `og:url`, `og:type`, `twitter:card="summary_large_image"`.
- **Favicons:** `favicon.ico`, an SVG icon, `apple-touch-icon` (180×180). Remove framework default icons (Next/Vercel/Angular logos).
- Canonical URL (`<link rel="canonical">`), always the production domain.
- `robots.txt` and `sitemap.xml`. Next: `app/robots.ts` + `app/sitemap.ts`. Others: static files in `public/`.
- Production is indexable (no accidental `noindex`); preview/staging deployments are **not** indexed.
- Custom 404 page with a link back home. Correct status codes (a missing page returns 404, not 200).
- Clean, readable URLs. One `<h1>` per page (shared with accessibility).

### Framework notes
- **Next.js:** Metadata API (`metadata` / `generateMetadata`), `opengraph-image` file convention.
- **Angular:** `Title` and `Meta` services or a custom `TitleStrategy` per route. Without SSR/prerendering, link-preview bots only see `index.html` → put at least default OG tags into `index.html`, or enable SSR/prerendering.
- **Vanilla / SPA (Vite, React):** same problem — static default tags in `index.html`, or prerender.

## Extra mile
- JSON-LD structured data (e.g. `WebSite`, `SoftwareApplication`, `Person` on a portfolio site).
- Per-page OG images (generated, e.g. Next `ImageResponse`).
- `hreflang` if the site has several languages.
- Google Search Console for the portfolio domain.

## Report
Checklist table per page (title, description, OG complete, canonical, indexable) before/after. Link to a preview check.
