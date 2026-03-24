# AGENTS.md

## Project Overview

**rosbinsalgado55** is a professional QA Tester portfolio — a static site showcasing manual testing, test automation, and software quality control expertise.

## Repository Structure

```
rosbinsalgado55/
├── .devcontainer/
│   └── devcontainer.json   # Dev environment config (universal image)
├── README.md               # One-line project description
└── AGENTS.md               # This file
```

> The project is in early/empty state. No source files, build system, or framework have been added yet.

## Tech Stack

- **Type**: Static portfolio site (framework TBD)
- **Language**: TBD
- **Dev container**: `mcr.microsoft.com/devcontainers/universal:4.0.1-noble`

## Agent Guidelines

### General

- Match the existing code style and naming conventions once source files exist.
- Do not commit or push unless explicitly asked.
- Do not expose secrets, API keys, or environment variables.

### File Operations

- Read files before editing to understand context.
- Keep changes minimal and focused on the task.
- Clean up temporary files before completing a task.

### Git

- Follow the single-commit history style (short, imperative messages).
- Stage only files relevant to the current task.
- Add co-author tag: `Co-authored-by: Ona <no-reply@ona.com>`

### Dev Container

- The current image is the heavy universal image (~10 GB).
- Prefer a lighter image once the tech stack is decided (e.g., `mcr.microsoft.com/devcontainers/javascript-node:24` for a Node-based site).

## What's Missing (known gaps)

- No source code, framework, or build tooling.
- No `.gitignore` (needed before any `npm install` / dependency install).
- No CI/CD configuration.
- No test setup (ironic for a QA portfolio — add at least one example test).
- No skill files under `.ona/skills/` or `.cursor/rules/`.
- No linting or formatting config.
