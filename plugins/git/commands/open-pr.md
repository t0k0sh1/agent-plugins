---
allowed-tools: Bash(gh pr ready:*), Bash(gh pr view:*)
description: Mark a draft PR as ready for review
---

## Context

- Current branch: !`git branch --show-current`
- PR info: !`gh pr view --json number,title,url,isDraft 2>/dev/null || echo "No PR found"`

## Your task

Mark a draft pull request as ready for review. User input: $ARGUMENTS

### Step 1: Identify PR

- If the user specified a PR number (e.g. `#123` or `123`), use that number
- Otherwise, use the PR associated with the current branch (from the Context above)
- If no PR is found, display the following and stop:
  > No PR found. Run this command on a branch with an associated PR, or specify a PR number.

### Step 2: Check draft status

- If the PR is not a draft, display the following and stop:
  > PR #<number> is already open (not a draft).

### Step 3: Open PR

Run `gh pr ready <number>` to mark the PR as ready for review.

### Step 4: Report

Display a confirmation message including the PR number, title, and URL.

You can call multiple tools in a single response. Do not use any other tools or perform any other operations.
