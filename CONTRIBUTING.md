# Contributing to MCP Wrapper Plugin

Thank you for your interest in contributing to the MCP Wrapper Plugin.

## Getting Started

1. Fork the repository
2. Clone your fork locally
3. Create a feature branch from `main`

## Development Setup

The plugin is located at:
```
~/.claude/plugins/claude-mcp-wrapper/
```

No build step is required. Changes to markdown files take effect immediately in Claude Code.

## Project Structure

```
claude-mcp-wrapper/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata
├── commands/                 # Slash commands
│   ├── analyze.md
│   ├── convert.md
│   ├── generate.md
│   └── scaffold.md
├── agents/                   # Subagent definitions
│   ├── mcp-analyzer.md
│   ├── schema-optimizer.md
│   └── wrapper-generator.md
├── skills/                   # Auto-activating skills
│   ├── analyze-mcp/
│   ├── create-help-system/
│   ├── generate-wrapper/
│   └── optimize-schema/
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## Making Changes

### Adding a New Command

1. Create a new `.md` file in `commands/`
2. Follow the YAML frontmatter format from existing commands
3. Document the command in README.md
4. Add entry to CHANGELOG.md

### Adding a New Subagent

1. Create a new `.md` file in `agents/`
2. Define role, tools, and permission mode
3. Document in README.md
4. Add entry to CHANGELOG.md

### Adding a New Skill

1. Create a new directory in `skills/`
2. Add `SKILL.md` with proper structure
3. Document in README.md
4. Add entry to CHANGELOG.md

## Code Style

- Use clear, descriptive names
- Follow existing patterns in the codebase
- Keep markdown well-formatted
- Include examples in documentation

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add new command for X
fix: resolve issue with Y
docs: update README with Z
refactor: simplify agent logic
test: add tests for command
chore: update dependencies
```

## Pull Request Process

1. Update CHANGELOG.md with your changes under `[Unreleased]`
2. Update README.md if adding new features
3. Ensure all documentation is accurate
4. Submit PR with clear description of changes

## Reporting Issues

When reporting issues, please include:

1. Description of the problem
2. Steps to reproduce
3. Expected behavior
4. Actual behavior
5. Claude Code version

## Questions

For questions about contributing, open an issue with the `question` label.
