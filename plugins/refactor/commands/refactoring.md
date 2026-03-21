---
allowed-tools: Read, Edit, Glob, Grep
description: Simplify and refactoring code — detect duplication, unnecessary complexity, and naming issues, then apply improvements
---

## Context

- Current branch: !`git branch --show-current`
- Current git status: !`git status`

## Your task

Analyze the target code for refactoring opportunities, propose a plan, and apply improvements after user confirmation.

### Step 1: Identify the target

Ask the user which files or directories to refactor. If the user has already specified a target, proceed with that.

Use Glob and Grep to locate and read the relevant source files.

### Step 2: Analyze the code

Read each target file and identify refactoring opportunities in these categories:

1. **Duplication** — Repeated logic that can be extracted into shared functions or modules.
2. **Unnecessary complexity** — Overly nested conditions, convoluted control flow, or code that can be simplified.
3. **Naming issues** — Unclear, misleading, or inconsistent variable/function/class names.
4. **Dead code** — Unused imports, unreachable branches, or commented-out code.
5. **Structure** — Long functions, god classes, or poor separation of concerns.

### Step 3: Present the refactoring plan

Show a summary table of all findings:

| # | Category | File | Finding | Proposed change |
|---|----------|------|---------|-----------------|
| 1 | Duplication | ... | ... | ... |
| 2 | Complexity | ... | ... | ... |

For each item, explain **why** the change improves the code and note any risks.

### Step 4: Apply changes

Wait for the user to confirm which items to apply. Then:

- Read the target file before every edit.
- Apply changes one at a time using Edit.
- Preserve existing behavior — refactoring must not change functionality.

### Step 5: Report

After all changes are applied, provide a summary:

- Number of refactorings applied
- Number of skipped items
- List of modified files
- Any follow-up suggestions the user may want to consider later
