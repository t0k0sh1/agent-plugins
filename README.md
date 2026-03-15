# agent-plugins

A collection of Claude Code plugins that enhance your development workflow with git operations, semantic code analysis, and rule generation.

## Plugins

| Plugin | Description |
|--------|-------------|
| **git** | Streamline your git workflow with simple commands for committing, pushing, and creating pull requests |
| **rule-creator** | Generate project-specific Claude Code rules (.claude/rules/) by analyzing the codebase |
| **serena** | Provide codebase-aware assistance using Serena for semantic code analysis and navigation |
| **review** | Run CodeRabbit code reviews and apply suggested fixes |

## Installation

Add the marketplace and install plugins:

```bash
claude marketplace add t0k0sh1/agent-plugins
claude plugin add git
claude plugin add rule-creator
claude plugin add serena
claude plugin add review
```

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
