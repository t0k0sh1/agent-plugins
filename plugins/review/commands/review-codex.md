---
allowed-tools: Bash(which:*), Bash(codex:*), Read, Edit
description: Run an OpenAI Codex CLI code review, triage findings, and apply fixes
---

## Context

- Current branch: !`git branch --show-current`
- Current git status: !`git status`

## Your task

Perform an OpenAI Codex CLI code review on the current changes, triage the findings, and apply fixes.

### Step 1: Check CLI availability

Run `which codex` to verify the Codex CLI is installed. If not found, inform the user with:

> Codex CLI (`codex`) がインストールされていません。
> https://github.com/openai/codex を参照してインストールしてください。

Then stop — do not proceed further.

### Step 2: Run the review

Execute `codex review` to get review results.

### Step 3: Triage findings

Classify each finding into one of three categories:

1. **Auto-fix** — Clear bugs, security issues, type errors, or other objectively incorrect code. Apply these fixes automatically without asking.
2. **Needs confirmation** — Design decisions, style changes, or suggestions with trade-offs. Present these to the user for approval before applying.
3. **Skip** — Nitpicks, subjective preferences, or cosmetic suggestions. Skip these silently.

### Step 4: Present triage summary

Show the user a summary table of all findings with their categories:

| # | Category | File | Finding |
|---|----------|------|---------|
| 1 | Auto-fix | ... | ... |
| 2 | Needs confirmation | ... | ... |
| 3 | Skip | ... | ... |

### Step 5: Apply fixes

- For **Auto-fix** items: Read the target file, then apply the fix using Edit. Do this immediately.
- For **Needs confirmation** items: Ask the user which ones to apply, then fix only the approved ones.
- For **Skip** items: Do nothing.

### Step 6: Report

After all fixes are applied, provide a summary:

- Number of auto-fixed items
- Number of user-approved fixes applied
- Number of skipped items
- List of modified files
