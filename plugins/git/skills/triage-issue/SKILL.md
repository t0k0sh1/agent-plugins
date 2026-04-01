---
name: triage-issue
description: "When an issue, bug, or review comment is found that may fall outside the current task scope, use this skill to ask the user whether to address it now or create a GitHub issue for later. Always use this skill instead of silently skipping or auto-deciding — the user decides what to include."
allowed-tools: Bash(gh issue create:*), Bash(gh repo view:*)
---

# Triage Issue

When you encounter a problem, bug, review comment, or improvement opportunity that may be outside the current task scope, **always** ask the user how to handle it. Never silently skip or auto-decide.

## When to use

- A review comment suggests a change unrelated to the current fix
- You discover a bug or code smell while working on something else
- A fix requires changes that extend beyond the original scope
- Any situation where you are unsure whether something should be addressed now

## Process

### Step 1: Present the issue

Clearly describe the issue to the user:

- **What**: What the problem or suggestion is
- **Where**: File and line (if applicable)
- **Context**: How you found it (review comment, code reading, etc.)

### Step 2: Ask the user

Present two options:

1. **Include in current fix** — Address it as part of the ongoing work
2. **Create a GitHub issue** — File it for later and continue with the current task

Wait for the user's response. Do not proceed until the user has made a choice.

### Step 3: Execute the choice

- **If including**: Continue with the fix as part of the current work. No further action needed from this skill.
- **If creating an issue**: Run `gh issue create` with:
  - A clear, descriptive title
  - A body that includes the context (what, where, how it was found)
  - Display the created issue URL to the user
