---
name: rule-creator
description: "Generate project-specific Claude Code rules (.claude/rules/) by analyzing the codebase. Use this skill when the user wants to create, add, or set up rules for their project, establish coding conventions, or configure path-scoped instructions for Claude."
---

# Rule Creator

A skill for generating project-specific Claude Code rules by analyzing the codebase and collaborating with the user.

## Overview

This skill analyzes a project's codebase to detect its technology stack, then proposes and generates appropriate `.claude/rules/` files through an interactive dialogue. Unlike skill creation, rules are declarative and always-on, so this follows a single-pass flow: analyze, propose, discuss, generate, review.

## Process

### Step 1: Analyze the codebase

Scan the project to understand its technology stack and conventions. Look for:

**Language & framework indicators:**

- `package.json` (Node.js — check for React, Next.js, Vue, Express, etc.)
- `tsconfig.json` (TypeScript)
- `go.mod` (Go)
- `Cargo.toml` (Rust)
- `pyproject.toml`, `requirements.txt`, `setup.py` (Python — check for Django, Flask, FastAPI, etc.)
- `Gemfile` (Ruby)
- `composer.json` (PHP — check for Laravel, Symfony, etc.)
- `CMakeLists.txt` (C/C++)
- `build.gradle`, `pom.xml` (Java/Kotlin)
- `*.swift`, `Package.swift` (Swift)

**Build & tooling indicators:**

- `.eslintrc*`, `biome.json`, `.prettierrc` (linting/formatting)
- `jest.config.*`, `vitest.config.*`, `pytest.ini`, `.rspec` (testing)
- `Dockerfile`, `docker-compose.yml` (containerization)
- `Makefile`, `justfile` (build automation)
- `.github/workflows/` (CI/CD)

**Existing Claude configuration:**

- `.claude/rules/` (existing rules — avoid duplication)
- `CLAUDE.md` (existing project instructions — complement, don't repeat)
- `.claude/settings.json` (permissions and tool config)

Report findings concisely to the user before proceeding.

### Step 2: Propose rule categories

Based on the analysis, propose relevant rule categories. Common categories include:

| Category            | When to propose              | Example paths                      |
| ------------------- | ---------------------------- | ---------------------------------- |
| Code style          | Always                       | `src/**/*.{ts,tsx}`                |
| Testing conventions | Test framework detected      | `tests/**/*`, `**/*.test.*`        |
| API design          | API routes/controllers found | `src/api/**/*`, `src/routes/**/*`  |
| Database            | ORM/migration files found    | `**/migrations/**`, `**/models/**` |
| Security            | Auth/crypto code found       | `src/auth/**/*`, `**/*.env*`       |
| Documentation       | Docs directory exists        | `docs/**/*`, `**/*.md`             |
| Component patterns  | UI framework detected        | `src/components/**/*`              |
| Build & CI          | CI config found              | `.github/**/*`, `Dockerfile`       |

Present the proposed categories with a brief explanation of what each rule would cover. Ask the user which ones they want, and if they have additional categories in mind.

### Step 3: Interactive refinement

For each selected category, ask one question at a time to understand specific preferences:

- What conventions does the team follow?
- Are there patterns to enforce or anti-patterns to prevent?
- Are there project-specific constraints?

Keep this lightweight — don't over-interview. If the codebase already reveals the conventions (e.g., existing ESLint config, test patterns), state what you found and ask for confirmation rather than asking from scratch.

### Step 4: Check for conflicts

Before generating, verify:

- No duplication with existing `.claude/rules/` files
- No contradiction with `CLAUDE.md` instructions
- `paths:` patterns actually match files in the project (run a quick glob check)

If conflicts are found, present them and ask how to resolve.

### Step 5: Generate rules

Generate each rule as a separate `.md` file in `.claude/rules/`. Follow this format:

```markdown
---
paths:
  - "relevant/glob/**/*.ext"
---

# Rule Title

- Concise, actionable instruction
- Explain the why, not just the what
- One topic per rule file
```

**Writing guidelines:**

- Keep rules concise — short, actionable bullet points
- Explain reasoning when it isn't obvious (the model responds better to understanding why)
- Use `paths:` to scope rules to relevant files — avoid global rules unless truly universal
- One `.md` file per topic for maintainability
- Name files descriptively: `code-style.md`, `testing.md`, `api-design.md`
- Avoid heavy-handed MUST/NEVER unless truly critical — explain the reasoning instead

### Step 6: Review with user

Present the generated rules to the user for review:

- Show each file's name, paths scope, and content
- Ask if anything needs adjustment
- Apply corrections if requested

Once approved, confirm the files are written and summarize what was created.

## Key Principles

- **Analyze first, ask second** — detect conventions from the codebase before asking the user
- **Complement, don't duplicate** — rules should add to CLAUDE.md, not repeat it
- **Scope tightly** — use `paths:` to keep rules relevant and save context tokens
- **One file, one topic** — modular rules are easier to maintain
- **Explain the why** — rules with reasoning are more effective than rigid commands
