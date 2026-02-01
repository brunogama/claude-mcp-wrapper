---
name: analyze-mcp
description: Analyze MCP server structure and create wrapping strategy when user needs to understand an MCP server or create a CLI wrapper
context: fork
agent: mcp-analyzer
allowed-tools:
  - Read
  - Bash
  - Grep
  - Glob
---

# Analyze MCP Server Structure

This skill analyzes an MCP server configuration and generates a comprehensive wrapping strategy.

## When This Skill Activates

Use this skill when:
- User asks about analyzing an MCP server
- User wants to understand MCP server capabilities
- User needs a wrapping strategy for an MCP server
- User requests tool inventory for an MCP
- Preparing to create a CLI wrapper

## What This Skill Does

1. Reads MCP server configuration from Claude Code
2. Introspects available tools and their signatures
3. Groups tools into logical namespaces
4. Designs 4-level progressive disclosure help structure
5. Recommends detail level enums for optimization
6. Estimates token savings potential
7. Generates implementation roadmap

## Usage

```
User: "Analyze the GitHub MCP server"
User: "What tools does the firecrawl MCP provide?"
User: "Create a wrapping strategy for the ref MCP"
```

## Output

Generates a comprehensive analysis report with:
- Tool inventory grouped by namespace
- Progressive disclosure structure (4 levels)
- Detail level recommendations
- Token efficiency estimates
- Implementation roadmap

## Example Output

```markdown
# MCP Server Analysis: github

## Server Overview
- Name: github
- Transport: stdio
- Tools Count: 15
- Configuration: ~/.config/claude/mcp.json

## Tool Inventory

### Namespace: repository
- **repository.get**: Get repository information
- **repository.list**: List user repositories
- **repository.create**: Create new repository

### Namespace: issue
- **issue.list**: List repository issues
- **issue.create**: Create new issue
- **issue.update**: Update issue

## Progressive Disclosure Structure
[4-level help outline]

## Token Efficiency Estimate
- Traditional approach: ~80,000 tokens per complex workflow
- Code API approach: ~5,000 tokens (93.8% savings)

## Implementation Roadmap
[Step-by-step plan]
```

## Agent Context

This skill runs in a forked context using the `mcp-analyzer` agent, which has specialized knowledge of:
- MCP protocol introspection
- Tool categorization patterns
- Progressive disclosure design
- Token efficiency optimization
- CLI wrapper patterns from ~/cli-wrappers/
