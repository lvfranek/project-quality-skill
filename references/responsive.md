# Responsive Design

## Measure
Check every page at these widths: **320, 375, 390, 768, 1024, 1280, 1440, 1920** px, plus phone landscape (e.g. 844×390) and one very wide screen (2560 px). Use browser devtools or Playwright screenshots. At least once on a real phone.

## Essential
- **No horizontal scrolling** at any width. Find the culprit (fixed widths, long words/URLs, images, tables, `100vw` with scrollbar).
- Mobile-first CSS, relative units (`rem`, `%`, `fr`), fluid type with `clamp()` where it helps. No layout that only works at one exact width.
- Content has a sensible `max-width` on large screens — lines of text max. ~75 characters, layout doesn't stretch endlessly at 2560 px.
- Images and media: `max-width: 100%; height: auto;`. Wide tables scroll inside their own container (`overflow-x: auto`).
- Long words and URLs wrap: `overflow-wrap: anywhere` where needed.
- Mobile viewport height: use `dvh`/`svh` instead of `100vh` for full-height sections (the mobile browser bar otherwise cuts content).
- Input font size ≥ 16 px (otherwise iOS zooms in on focus).
- Touch targets ≥ 44×44 px with enough space between them.
- Hover-only features have a tap alternative. Hover effects inside `@media (hover: hover)`.
- Mobile navigation: works by tap and keyboard, closes on link click and on Esc, page behind doesn't scroll while it's open.
- The on-screen keyboard doesn't cover the focused input or the submit button.
- Fixed/sticky bars respect notches: `env(safe-area-inset-*)` with `viewport-fit=cover` if used.
- `<meta name="viewport" content="width=device-width, initial-scale=1">` — never block zooming (`user-scalable=no`, `maximum-scale=1`).

## Extra mile
- Container queries for components that live in different widths (cards in sidebar vs. main).
- Playwright screenshot tests for key pages at 3 widths.
- Responsive images (`srcset`/`sizes` or the framework's image component) — see performance.

## Report
Table: page × width → OK / problem (with screenshot description). Fixes in one sentence each.
