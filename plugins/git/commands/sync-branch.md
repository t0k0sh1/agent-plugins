---
allowed-tools: Bash(git fetch:*), Bash(git merge:*), Bash(git branch:*), Bash(git log:*)
description: Sync current branch with the latest changes from the target branch
---

## Context

- Current branch: !`git branch --show-current`
- Available remote branches: !`git branch -r`

## Your task

Merge the latest changes from the target branch **into the current branch**. Note: this merges target into the current branch, NOT the other way around.

Direction: `origin/<target> → current branch (!`git branch --show-current`)`

1. Extract the target branch name from the user's prompt. Default to `main` if not specified.
2. **Safety check**: If the current branch is the same as the target branch, report "Already on the target branch — no sync needed" and stop.
3. Run `git fetch origin` to get the latest remote state.
4. Run `git merge origin/<target>` to merge the target branch changes into the current branch.
5. **On conflict**: Report the conflict to the user and let them decide how to proceed. Do NOT auto-abort the merge.
6. **On success**: Report the merge result and a summary including the number of commits merged.

Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls and the result summary.
