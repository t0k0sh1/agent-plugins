---
allowed-tools: Bash(gh pr:*)
description: Merge a pull request
---

## Context

- Current branch: !`git branch --show-current`
- Current branch PR info: !`gh pr view --json number,title,state,mergeable,mergeStateStatus 2>/dev/null || echo "No PR found"`

## Your task

Merge a pull request. The user's prompt is: $ARGUMENTS

1. Determine the target PR:
   - If the user specified a PR number (e.g. `#123` or `123`), use that number
   - Otherwise, use the PR associated with the current branch (from the context above)
   - If no PR is found, inform the user and stop

2. Check the PR status using `gh pr view <PR> --json state,mergeable,mergeStateStatus,title,number`:
   - If `state` is not `OPEN`, inform the user that the PR is not open and stop
   - If `mergeable` is not `MERGEABLE`, inform the user that the PR cannot be merged and show the reason (`mergeStateStatus`) and stop

3. If the PR is mergeable, execute `gh pr merge <PR> --merge --delete-branch` to merge it

4. Report the result to the user (PR number, title, and whether the merge succeeded)
