# agent-plugins

A collection of Claude Code plugins that enhance your development workflow with git operations, project scaffolding, GitHub integration, semantic code analysis, and structured brainstorming.

## Plugins

| Plugin | Description |
|--------|-------------|
| **git** | Streamline your git workflow with simple commands for committing, pushing, and creating pull requests |
| **create** | Create, evaluate, and iteratively improve Claude Code skills with test-driven feedback loops and benchmarking |
| **github** | Interact with GitHub repositories, issues, pull requests, and more through the GitHub MCP server |
| **serena** | Provide codebase-aware assistance using Serena for semantic code analysis and navigation |
| **thinking** | Brainstorm ideas into designs and implementation plans through collaborative dialogue before any creative or implementation work |

## Installation

Install a plugin using the `claude plugin add` command:

```bash
claude plugin add /path/to/agent-plugins/plugins/<plugin-name>
```

For example:

```bash
claude plugin add /path/to/agent-plugins/plugins/git
claude plugin add /path/to/agent-plugins/plugins/thinking
```

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
