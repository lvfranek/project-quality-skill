---
name: portfolio-quality
description: Audits and fixes web projects (Next.js, Angular, React, Vue, Svelte, vanilla JS) until they are portfolio-ready for senior reviewers — README, accessibility, responsive design, performance, SEO, code quality (ESLint, TypeScript, tests, GitHub Actions CI), security (secrets, Supabase RLS, rate limiting, headers) and German legal pages (Impressum, Datenschutzerklärung, cookies). Use this skill whenever the user wants to audit, polish, clean up, check or "make portfolio-ready" a project, write or restructure a README, or asks about any single one of these areas in their own project — even if they don't mention the word portfolio.
---

# Portfolio Quality

Goal: a senior developer opens the repo and the live site and finds nothing to criticize.
Not "as many features as possible" — **correct, consistent, honest, working**.

## 1. Understand the project first

Before changing anything, read `package.json`, the lockfile, framework config, existing lint/test/CI setup, deployment config (`vercel.json`, `netlify.toml`, …) and the backend (Supabase, Firebase, own API).
Summarize in 3–5 lines: stack, rendering (SSR / static / SPA), backend, hosting, what already exists. Ask the user only about things you cannot find out yourself.

## 2. Pick the scope

If the user names one area, load only that reference file. For a **full audit**, run the phases in this order, **one phase at a time**, and stop after each phase so the user can review:

| # | Phase | File | Why this position |
|---|---|---|---|
| 1 | Code quality, tests, CI | `references/code-quality.md` | Baseline first, so every later fix can be verified |
| 2 | Accessibility | `references/accessibility.md` | Biggest quality signal, touches markup |
| 3 | Responsive design | `references/responsive.md` | Builds on the markup fixes |
| 4 | Performance | `references/performance.md` | Measure after markup/CSS changes |
| 5 | SEO & sharing | `references/seo.md` | Metadata, previews |
| 6 | Security | `references/security.md` | Secrets, backend, headers |
| 7 | Legal (Germany) | `references/legal-de.md` | Needs the final list of services. Tailored to German law — skip or replace with local requirements for projects/users outside Germany |
| 8 | README & repo polish | `references/readme.md` | Last, because it describes the final state |

If the user only wants a quick check: run the **Essential** items of every phase as a read-only audit and deliver one combined report, without fixing.

## 3. Rules for every phase

- **Measure → fix → re-measure → report.** Never claim a fix works without checking it.
- **Essential** items must be done. **Extra mile** items: suggest them; only do them if the user agrees or they take a few minutes.
- Test against a **production build** served locally, not the dev server.
- Keep the design. Change visuals only as much as a fix requires.
- Don't add dependencies without saying why. Prefer the platform and the framework's built-ins.
- Lint, typecheck, tests and build must pass after every phase.
- Small commits per topic with conventional messages (e.g. `fix(a11y): add labels to icon buttons`). Only commit if the user allowed it.
- Never invent results. If you can't run something (no browser, page behind a login, no access to the dashboard), say so and tell the user exactly how to check it themselves.
- Never print, log or commit secrets.
- Skip what doesn't apply (e.g. no backend → no rate limiting) and say so in one line.

## 4. Report after each phase

1. Before/after table (scores, number of issues)
2. What was fixed — one simple sentence each
3. What's open or needs a manual check, with instructions
4. Extra-mile suggestions (max. 5, most valuable first)
