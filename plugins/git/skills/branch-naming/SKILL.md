---
name: branch-naming
description: "Create a new git branch with a well-named branch following the convention. Use this skill whenever the user wants to create a new branch, switch to a new branch, or start working on a new feature/fix/task."
allowed-tools: Bash(git checkout -b:*), Bash(git branch:*), Bash(git status:*)
---

# Branch Naming

Create a new git branch following the `<type>/<short-description>` naming convention.

## Branch Name Format

```
<type>/<short-description>
```

- **type**: one of the Conventional Commits types listed below
- **short-description**: concise English summary in kebab-case (2-4 words ideal)

## Types

| Type | When to use |
|------|-------------|
| `feat` | New feature or functionality |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, whitespace, no code change |
| `refactor` | Code restructuring without behavior change |
| `test` | Adding or updating tests |
| `chore` | Build, CI, dependencies, tooling |

## Examples

- `feat/add-user-auth`
- `fix/null-pointer-crash`
- `docs/update-readme`
- `refactor/extract-parser`
- `chore/bump-dependencies`
- `test/add-api-tests`

## Your task

1. Check the current branch with `git branch --show-current`
2. Determine the type from the user's intent or the changes in progress
3. Generate a short, descriptive kebab-case summary
4. Run `git checkout -b <type>/<short-description>`
