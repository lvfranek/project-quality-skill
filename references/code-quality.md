# Code Quality, Tests & CI

This phase comes first: it creates the safety net (lint, typecheck, tests, build) that every later phase uses.

## Measure
Run and record: lint (errors/warnings), typecheck, tests (pass/fail), build (success, warnings), `npm audit` (by severity), unused code/deps (`npx knip`).

## Essential

### Clean code base
- **ESLint: 0 errors, 0 warnings** with the framework's config (Next: `eslint-config-next`; Angular: `angular-eslint`; others: `@eslint/js` recommended + `typescript-eslint`). Every `eslint-disable` has a comment explaining why.
- **Prettier** for formatting (+ `eslint-config-prettier` so they don't fight). Whole repo formatted once.
- **TypeScript `strict: true`**, no `any` without reason, `typecheck` script (`tsc --noEmit`; Angular: the build does it).
- Remove: `console.log`, commented-out code, unused files/exports/dependencies, framework boilerplate (default assets, default README text, empty or failing default `.spec` files).
- TODOs: fix them, or move them to "Known Issues" in the README.
- Consistent naming and folder structure; no files with 1000+ lines if they can be split sensibly.

### Repo hygiene
- `.gitignore` covers `node_modules`, build output, `.env*` (except `.env.example`), OS/editor files. None of these are committed.
- Lockfile committed, **one** package manager.
- `.env.example` lists every variable with placeholder values.
- Node version pinned (`.nvmrc` and/or `"engines"` in `package.json`).
- Scripts in `package.json`: `dev`, `build`, `start`/`preview`, `lint`, `typecheck`, `test`, `format`.
- `npm audit`: fix high/critical where possible; explain what's left.

### Tests (realistic, not maximal)
Quality over coverage. No coverage percentage targets, no snapshot spam.
- **Unit tests** for pure logic: utils, validation, formatting, state/reducers. Runner: Vitest (Vite, Next, React, Vue) or the runner the Angular project already uses.
- **Component tests** for 2–3 key components with Testing Library (Angular Testing Library for Angular): test what the user sees and does, not implementation details.
- Tests are deterministic and don't hit real external services (mock them, or use a separate test project).

### CI with GitHub Actions
One workflow that runs on every push to `main` and on every pull request: install → lint → typecheck → test → build. Add the status badge to the README.

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:

concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version-file: .nvmrc
          cache: npm
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
      - run: npm test -- --run   # non-watch mode; adjust per runner
      - run: npm run build
        env:
          # placeholders or GitHub secrets, never real values in the file
          NEXT_PUBLIC_SUPABASE_URL: ${{ secrets.SUPABASE_URL }}
          NEXT_PUBLIC_SUPABASE_ANON_KEY: ${{ secrets.SUPABASE_ANON_KEY }}
```
Adapt package manager, env variable names and test command to the project. Check that the action versions are current.

## Extra mile
- **1–3 E2E tests** with Playwright for the main user flow (e.g. sign in → create task → task appears), run in a CI job (with `npx playwright install --with-deps`). This is the test that impresses the most, but it's real extra infrastructure (browser install, potential flakiness) on top of unit/component tests — worth it, not a minimum.
- Lighthouse CI — one `lighthouserc.json` that covers accessibility, best practices, SEO and performance in a single setup (thresholds: accessibility ≥ 95, performance ≥ 90).
- Dependabot (`.github/dependabot.yml`, weekly, grouped minor/patch updates, also for GitHub Actions).
- Pre-commit hooks: `husky` + `lint-staged` (lint + format staged files).
- Branch protection on `main`: CI must pass before merge.
- Work in small pull requests with a short description — recruiters do look at PR history.

## Report
Table before/after: lint errors/warnings, typecheck errors, tests passing, build status, audit findings, unused deps/files removed.
