# Accessibility (target: WCAG 2.2 AA)

Automated tools find only ~30–40 % of problems. The manual checks below are **not optional**.

## Measure

1. Production build, served locally (`next build && next start`, `ng build` + static server, `vite build && vite preview`, …).
2. List all public pages you will check. Pages behind a login: check them too if a test account exists, otherwise list them as "not checked".
3. Lighthouse CLI on every page, mobile (default) and desktop (`--preset=desktop`), JSON output to get the exact failing elements:
   `npx lighthouse <url> --only-categories=accessibility,best-practices,seo --output=json --output-path=./lh-<page>-<device>.json --chrome-flags="--headless"`
4. axe on the same pages: `npx @axe-core/cli <url>` — catches things Lighthouse misses.
5. Do the manual checks below.

## Essential

### Structure & semantics
- Landmarks: one `<header>`, `<nav>`, one `<main>`, `<footer>`. More than one `<nav>` → give each an `aria-label` ("Main", "Footer").
- Exactly one `<h1>` per page. Headings don't skip levels (h2 → h4). Headings are for structure, not for font size.
- **Buttons do things, links go places.** No clickable `<div>`/`<span>`. Every `<a>` has an `href`.
- Lists are `<ul>`/`<ol>`, tabular data is a `<table>` with `<th scope>`.
- `<html lang="…">` matches the content language. Every page has a unique, descriptive `<title>`.
- Native elements before ARIA. "No ARIA is better than bad ARIA."

### Keyboard — the Tab walk
Walk every page with **Tab, Shift+Tab, Enter, Space, Esc and arrow keys**, once at desktop width and once at ~375 px (with the mobile menu open and closed). Write down every focus stop in order. Check:

- **Skip link:** the first Tab stop is "Skip to main content", visible when focused, and jumps to `<main id="main">` (focus actually lands there).
- **Everything reachable:** every interactive element can be reached and used. Drag & drop, sliders, custom dropdowns need a keyboard way (e.g. "Move to…" buttons).
- **Logical order:** focus order = visual order, left→right, top→bottom. No positive `tabindex`. Watch out for CSS that reorders visually (`order`, `row-reverse`, absolute positioning).
- **Always visible focus:** never `outline: none` without a replacement. Use `:focus-visible` with a clear ring (e.g. `outline: 2px solid <darker brand color>; outline-offset: 2px`) with at least 3:1 contrast to the background.
- **Nothing invisible is focusable:** closed menus, off-canvas panels, hidden carousel slides, collapsed accordions, closed modals. Use `hidden`, `display: none` or `inert` — `opacity: 0` or `transform` alone keep elements focusable.
- **Focus not hidden:** a sticky header must not cover the focused element (`scroll-padding-top`).
- **Dialogs/modals:** focus moves into the dialog when it opens, stays inside (trap), Esc closes it, focus returns to the button that opened it. Prefer native `<dialog>` + `showModal()`.
- **Menus/dropdowns:** Esc closes and returns focus to the toggle button.
- **SPA route changes** (Next, Angular, React Router): after navigation, focus goes to the new page's `<h1>` or `<main>` and the page change is announced to screen readers. Check this, don't assume it.
- **Custom widgets** (tabs, combobox, menu) follow the keyboard patterns of the WAI-ARIA Authoring Practices — or are replaced by native elements.
- **No keyboard traps** anywhere (embeds, editors, maps).

### Names, labels & states
- Every input has a visible `<label>` connected via `for`/`id`. A placeholder is not a label.
- Required fields: `required` attribute + visible marker (not only a red color).
- Form errors: clear text next to the field, linked via `aria-describedby`, `aria-invalid="true"`, announced (`aria-live="polite"` or `role="alert"`). On submit with errors, focus moves to the first invalid field.
- Icon-only buttons/links have an accessible name: `aria-label="Close menu"`, `aria-label="GitHub profile"`. Decorative icons and SVGs: `aria-hidden="true"`.
- Images: meaningful `alt` text; decorative images `alt=""`. No "image of…".
- Link texts make sense on their own (no lone "click here" / "read more"). Links opening a new tab say so (visually hidden text or icon with label).
- States are announced: menu/FAQ/accordion toggles `aria-expanded` (+ `aria-controls`), toggle buttons `aria-pressed`, active nav link `aria-current="page"`. FAQ: `<details>`/`<summary>` is the simplest correct solution.
- Dynamic messages (toasts, "Saved", result counts, loading done) go into an `aria-live` region.

### Visual
- Contrast: normal text ≥ 4.5:1, large text (≥ 24 px, or ≥ 18.66 px bold) ≥ 3:1, UI parts (borders of inputs, icons, focus ring) ≥ 3:1. Fix with darker shades of existing colors, not new colors.
- Never color alone to convey meaning (errors, "not included", required fields, status). Add text, icon or pattern. Links inside body text are underlined.
- Page works at 200 % zoom and at 320 px width without horizontal scrolling.
- Click/tap targets at least 24×24 px (WCAG 2.5.8), better 44×44 px on touch.
- `prefers-reduced-motion`: reduce or turn off animations, parallax, autoplay.
- Anything that moves automatically for more than 5 seconds can be paused.

## Extra mile
- Screen-reader smoke test: VoiceOver (macOS, Cmd+F5) or NVDA (Windows). Navigate by headings, landmarks and form fields; open a menu and a dialog.
- Lint rules: `eslint-plugin-jsx-a11y` (React/Next) or `@angular-eslint` template accessibility rules.
- Automated a11y tests with `@axe-core/playwright` for the main pages, running in CI.
- Lighthouse CI — see `code-quality.md` Extra mile (one setup, covers accessibility together with best practices/SEO/performance).
- Dark mode via `prefers-color-scheme`, with checked contrast in both themes.

## Report
- Table: page × device → Lighthouse accessibility score and axe violations, before/after.
- The Tab-walk list per page, with problems marked.
- Each fix in one simple sentence.
- What couldn't be checked (e.g. pages behind a login) and how the user can test it.
