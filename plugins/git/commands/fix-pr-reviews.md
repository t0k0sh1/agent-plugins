---
allowed-tools: Bash(gh:*), Bash(git:*), Read, Edit
description: Fetch PR review comments, triage them, and apply fixes
---

## Context

- Current branch: !`git branch --show-current`
- PR info: !`gh pr view --json number,title,url,reviewDecision,isDraft 2>/dev/null || echo "No PR found"`

## Your task

Fetch PR review comments, triage them, and apply fixes. User input: $ARGUMENTS

### Step 1: Identify PR

- If the user specified a PR number (e.g. `#123` or `123`), use that number
- Otherwise, use the PR associated with the current branch (from the Context above)
- If no PR is found, display the following and stop:
  > No PR found. Run this command on a branch with an associated PR, or specify a PR number.

### Step 2: Fetch review comments

Get repository info with `gh repo view --json owner,name --jq '.owner.login + "/" + .name'` and call the following APIs:

1. `gh api repos/{owner}/{repo}/pulls/{number}/comments` — inline comments (attached to specific files/lines)
2. `gh api repos/{owner}/{repo}/pulls/{number}/reviews` — review summaries (general comments)
3. Fetch review threads with their GraphQL IDs (needed to resolve conversations later). Replace `{owner}`, `{repo}`, and `{number}` with the actual values obtained above:

```
gh api graphql -f query='
{
  repository(owner: "{owner}", name: "{repo}") {
    pullRequest(number: {number}) {
      reviewThreads(first: 100) {
        nodes {
          id
          isResolved
          comments(first: 100) {
            nodes {
              id
              databaseId
              body
              author { login }
            }
          }
        }
      }
    }
  }
}'
```

Match each REST API comment to its GraphQL thread using the comment's `id` (REST `id` == GraphQL `databaseId`). Note: this query fetches up to 100 threads and up to 100 comments per thread, which covers the vast majority of PRs.

If there are no review comments at all, display the following and stop:
> No review comments found.

### Step 3: Triage classification

Classify each comment into one of the following 3 categories:

1. **Auto-fix** — Comments with a `suggestion` block or objective bug reports (typos, type errors, missing null checks, etc.). Thread must not already be resolved.
2. **Needs confirmation** — Design changes, refactoring proposals, optimization trade-offs, or anything difficult to auto-judge. Thread must not already be resolved.
3. **Skip** — Already-resolved threads, LGTM/praise, or questions without specific fix suggestions.

### Step 4: Summary table

Display the classification results for all comments in a table:

| # | Category | Reviewer | File | Line | Summary |
|---|----------|----------|------|------|---------|
| 1 | Auto-fix | ... | ... | ... | ... |
| 2 | Needs confirmation | ... | ... | ... | ... |
| 3 | Skip | ... | ... | ... | ... |

### Step 5: Apply fixes

- **Auto-fix**: Read the target file with `Read` and apply the fix immediately with `Edit`. Then:
  1. Post a reply to the review thread explaining the fix:
     ```
     gh api repos/{owner}/{repo}/pulls/{number}/comments/{comment_id}/replies \
       -f body="Fixed: <brief description of what was changed>"
     ```
  2. Resolve the conversation thread using the GraphQL thread ID obtained in Step 2:
     ```
     gh api graphql -f query='
     mutation {
       resolveReviewThread(input: {threadId: "{thread_id}"}) {
         thread { id isResolved }
       }
     }'
     ```
- **Needs confirmation**: Ask the user which ones to apply, and only fix those that are approved. For each approved fix, apply the code change, post a reply, and resolve the thread using the same steps as Auto-fix above.
- **Skip**: Do nothing

### Step 6: Report

After all fixes are applied, display a summary including:

- Number of auto-fixed items
- Number of user-approved fixes
- Number of skipped items
- List of modified files
- Number of conversations resolved

**Important**: Do NOT commit or push changes. The user will do so explicitly.
