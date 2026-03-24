# AGENTS Improvement Spec

**Repo:** rosbinsalgado55  
**Date:** 2025-07  
**Status:** Draft

---

## Audit Summary

### What's good

| Item | Notes |
|------|-------|
| `devcontainer.json` exists | Environment is reproducible from day one. |
| README describes intent | One-liner is accurate and scoped. |
| Single clean commit | No noise in history. |

### What's missing

| Gap | Impact |
|-----|--------|
| No `AGENTS.md` | Agents have no project context, conventions, or guardrails. Created in this session — but it's skeletal because the project itself is empty. |
| No source code or framework | Nothing for an agent to work with or reason about. |
| No `.gitignore` | First `npm install` / `pip install` will pollute the repo with `node_modules` or `venv`. |
| No skill files (`.ona/skills/`, `.cursor/rules/`) | No reusable agent workflows for QA-specific tasks. |
| No CI/CD | No automated checks; agents can't verify their own changes. |
| No linting/formatting config | Agents will produce inconsistent style. |
| No test examples | A QA portfolio with zero tests is a credibility gap. |
| `devcontainer.json` uses universal image | ~10 GB image; slow cold starts; no justification once stack is chosen. |

### What's wrong

| Issue | Detail |
|-------|--------|
| `devcontainer.json` has no `postCreateCommand` | Dependencies aren't installed automatically; new environments start broken. |
| `devcontainer.json` has no `forwardPorts` | Preview servers can't be reached without manual port configuration. |
| README has no setup or run instructions | A contributor (or agent) can't get started without guessing. |

---

## Improvement Spec

### 1. Establish the tech stack (prerequisite for everything else)

**Decision needed:** Choose a framework for the portfolio site before any other work.

Recommended options:
- **Astro** — static-first, fast, good for portfolios, minimal JS by default.
- **Next.js** — more familiar, larger ecosystem, SSG mode works fine.
- **Plain HTML/CSS/JS** — simplest, no build step, appropriate if the portfolio is a single page.

Once decided, update `AGENTS.md` → Tech Stack section.

---

### 2. Add `.gitignore` before installing any dependencies

Create `.gitignore` appropriate for the chosen stack **before** running any install command.

Minimum entries regardless of stack:
```
.env
.env.local
.DS_Store
*.log
```

Node-specific additions:
```
node_modules/
dist/
.next/
.astro/
```

---

### 3. Right-size the dev container

Replace the universal image with a stack-specific one and add lifecycle hooks.

**Example for a Node/Astro stack:**
```jsonc
{
  "name": "rosbinsalgado55",
  "image": "mcr.microsoft.com/devcontainers/javascript-node:24",
  "postCreateCommand": "npm install",
  "forwardPorts": [4321],
  "portsAttributes": {
    "4321": { "label": "Dev server", "onAutoForward": "openPreview" }
  }
}
```

---

### 4. Expand `AGENTS.md` with real conventions

Once source files exist, update `AGENTS.md` to include:

- **Directory map** — where pages, components, styles, and assets live.
- **Naming conventions** — file names, component names, CSS class patterns.
- **Build & run commands** — `npm run dev`, `npm run build`, `npm run preview`.
- **Test commands** — how to run any automated tests.
- **Commit message format** — e.g., `feat:`, `fix:`, `chore:` prefixes if using Conventional Commits.
- **Do-not-touch list** — files agents should never modify (e.g., generated files, lock files).

---

### 5. Add a QA-focused skill file

Create `.ona/skills/qa-portfolio.md` with reusable agent workflows relevant to this repo:

- **Add a new project case study** — steps to scaffold a new portfolio entry (copy template, fill metadata, add screenshots).
- **Write a test plan** — template for documenting manual test cases in Markdown.
- **Add a Playwright smoke test** — steps to add an end-to-end test for a new page.
- **Accessibility audit** — run `axe` or `pa11y` against the dev server and report findings.

---

### 6. Add CI via GitHub Actions

Create `.github/workflows/ci.yml` with at minimum:

```yaml
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 24 }
      - run: npm ci
      - run: npm run build
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 24 }
      - run: npm ci
      - run: npm run lint
```

Add a test job once tests exist.

---

### 7. Add at least one automated test

A QA portfolio should demonstrate testing skills. Add one of:

- **Playwright smoke test** — visit the home page, assert the page title and at least one heading.
- **Vitest unit test** — test a utility function (e.g., date formatter, tag filter).
- **Lighthouse CI** — assert performance and accessibility scores above a threshold.

Minimum bar: one passing test that runs in CI.

---

### 8. Add linting and formatting

```bash
npm install --save-dev eslint prettier
```

Add `eslint.config.js` and `.prettierrc`. Add scripts to `package.json`:

```json
"lint": "eslint src",
"format": "prettier --write src"
```

Agents will use `npm run lint` to self-verify changes.

---

## Priority Order

| Priority | Item |
|----------|------|
| 1 | Decide tech stack |
| 2 | Add `.gitignore` |
| 3 | Scaffold source code |
| 4 | Right-size devcontainer |
| 5 | Expand `AGENTS.md` |
| 6 | Add linting/formatting |
| 7 | Add CI |
| 8 | Add at least one test |
| 9 | Add `.ona/skills/qa-portfolio.md` |
