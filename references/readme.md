# README & Repo Polish

All project READMEs follow the same structure so they look like one consistent portfolio. Write in **English**, short, concrete and honest. Follow these rules exactly.

## Required structure (always in this order)

1. `# <emoji> Project Name` — one fitting emoji. Optional badges directly below (e.g. CI status, license, deploy status). Max. 3–4 badges, only ones that show something real.
2. **Description** — 2–5 sentences in plain language: what the project does and who it's for.
3. **Screenshot** — `![Project Name](path/to/screenshot.png)`. Current, sharp, showing the app's main view. A short GIF/video of the main flow is even better.
4. `## Tech Stack` — bullet list of technologies in backticks for small projects, or a table (Area | Choice) for bigger ones.
5. `## Features` — bullet list. Format: `- **Feature name** — short explanation.` For bigger projects, group with bold subheadings (e.g. **On your page**, **In the dashboard**).
6. `## Live Demo` — just the link, e.g. `[my-app.vercel.app](https://my-app.vercel.app/)`. Mention a guest/demo login if the app has one. For VS Code extensions use `## Install` with the Marketplace link instead.
7. `## ⚙️ Installation` — how to run it locally, max. 10 steps:
   - **Prerequisites** (e.g. Node.js version, accounts needed)
   - Numbered steps with `bash` code blocks for a minimal working setup
8. `## License` — always last. E.g. "MIT — see [LICENSE](LICENSE)". If the code should not be reused: "All rights reserved."

## Optional sections (only if useful, in this order, between Installation and License)

- `## Environment Variables` — table: Variable | Description | Required. Names and placeholders only.
- `## Usage` — more detail than Installation: main workflows, optional commands, configuration options.
- `## Available Scripts` — table: Command | What it does.
- `## Architecture` — how the parts work together (a short diagram, e.g. Mermaid, is welcome).
- `## Folder Structure` — short tree of the important folders only, with one-line comments.
- `## Tests` — what is tested, briefly, and the commands to run the tests.
- `## Deployment`
- `## Security Considerations` — e.g. RLS, auth, rate limiting, headers. What was done, briefly.
- `## Limitations` — what the project deliberately doesn't do, or design decisions and their trade-offs.
- `## Known Issues` — known bugs or unfinished parts.
- `## Contributing` — only if contributions are wanted.
- `## Learn More` — links to docs, articles, related projects.

Project-specific sections (e.g. `## Admin Panel`, `## Automation`) are fine. Put them between Installation and License, where they fit best.

**Table of contents:** only if the README has more than ~6 sections. Put it as `## Table of Contents` after the screenshot. (GitHub also shows an automatic outline, so short READMEs don't need one.)

## Rules
- Use exactly these heading names. "Tech Stack", not "Technologies"; "Installation", not "Getting Started" or "Setup"; "Deployment", not "Deploying to Vercel".
- No emojis in `##` headings, except `## ⚙️ Installation`.
- No `---` horizontal rules.
- Short, concrete, honest. Only list features that are actually built; mark unfinished ones as "coming soon" or leave them out.
- **No sensitive data:** never real secrets, passwords, API keys or customer data. Only variable names and placeholders from `.env.example`.
- When restructuring an existing README: keep its content. Reorder and rename, but don't rewrite good text.
- Check that all commands in Installation actually work on a fresh clone.
- Check that image paths, links and internal anchor links work.

## Example (top part of a finished README)

```markdown
# 📋 Join

![CI](https://github.com/<user>/join/actions/workflows/ci.yml/badge.svg)

A task manager inspired by the Kanban system. Create and organize tasks using drag and drop, assign users and categories to keep your team aligned and productive.

![Join Kanban Board](public/join.png)

## Tech Stack
- `Angular`
- `TypeScript`
- `SCSS`
- `Supabase`

## Features
- **Kanban board** — create, edit and organize tasks in status columns.
- **Drag and drop** — move tasks between columns (also by keyboard).
- **Assignments** — assign users and categories to tasks.
- **Auth & sync** — login and data persistence powered by Supabase.

## Live Demo
[join-phi-lemon.vercel.app](https://join-phi-lemon.vercel.app/) — use "Guest login" to try it without an account.

## ⚙️ Installation
...
```

## Repo polish (GitHub)
- "About" section: one-line description, website link (live demo), 3–6 topics (e.g. `angular`, `supabase`, `kanban`).
- Social preview image set (Settings → Social preview, 1280×640).
- `LICENSE` file matches the License section.
- Default branch clean, no leftover branches with experiments, meaningful recent commit messages.
- Best projects pinned on the GitHub profile.

## Report
The finished README, plus a list of what you changed and anything the user must provide (screenshot, demo link, license choice).
