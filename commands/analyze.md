---
description: Analyze an MCP server and generate wrapping strategy
argument-hint: <mcp-server-name>
allowed-tools:
  - Read
  - Bash
  - Grep
  - Glob
---

# Analyze MCP Server

Analyzes an existing MCP server configuration and generates a comprehensive wrapping strategy following progressive disclosure patterns.

## Usage

This command analyzes an MCP server from your Claude Code configuration and provides:

1. Tool inventory and categorization
2. Suggested namespace organization
3. Progressive disclosure help structure
4. Detail level enum recommendations
5. CLI wrapper generation plan

## Process

1. Reads MCP server configuration from Claude Code
2. Introspects available tools using MCP protocol
3. Analyzes tool signatures, parameters, and return types
4. Groups tools into logical namespaces
5. Generates 4-level help documentation structure
6. Recommends detail level enums for response optimization
7. Creates implementation roadmap

## Arguments

- `$ARGUMENTS[0]`: MCP server name (from your Claude configuration)
- `$ARGUMENTS[1]` (optional): Output format (json|markdown|summary)

## Examples

```bash
# Analyze the GitHub MCP server
/claude-mcp-wrapper:analyze github

# Get detailed JSON analysis
/claude-mcp-wrapper:analyze github json

# Quick summary only
/claude-mcp-wrapper:analyze firecrawl summary
```

## Output

The analysis report includes:

- **Tool Inventory**: All available tools with signatures
- **Namespace Groups**: Suggested organization (e.g., user.*, repo.*, issue.*)
- **Help Structure**: 4-level progressive disclosure outline
- **Detail Levels**: Recommended enum values for response filtering
- **Token Estimates**: Projected savings vs traditional approach
- **Implementation Plan**: Step-by-step wrapper generation roadmap

## Next Steps

After analysis, use:
- `/claude-mcp-wrapper:generate` to create the CLI wrapper code
- `/claude-mcp-wrapper:scaffold` to set up complete project structure
