---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git push:*), Bash(git commit:*), Bash(git fetch:*), Bash(git merge:*), Bash(gh pr view:*), Bash(git diff:*), Read, Edit
description: Commit and push
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`

## Your task

Commit, merge the base branch, and push.

### 1. Commit

Create a single commit with an appropriate message based on the changes.

### 2. Merge base branch (only if the current branch is not main)

If the current branch is main, skip this step and go to step 3.

1. Detect the base branch:
   - Run `gh pr view --json baseRefName -q .baseRefName` to get the base branch of the PR associated with the current branch
   - If no PR exists (command errors), default to `main`
2. Run `git fetch origin` to get the latest remote state
3. Run `git merge origin/<base branch>` to merge the base branch changes
4. **On conflict**:
   - Run `git diff --name-only --diff-filter=U` to list conflicting files
   - `Read` each conflicting file and examine the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
   - Use `Edit` to resolve the conflicts
   - Stage each resolved file with `git add`
   - Once all conflicts are resolved, run `git merge --continue` to complete the merge
   - If any conflict cannot be resolved, report to the user and stop (do NOT auto-abort the merge)

### 3. Push

Push the branch to origin.

You can call multiple tools in a single response. Do not use any other tools or perform any other operations.
