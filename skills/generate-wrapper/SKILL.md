---
name: generate-wrapper
description: Generate production-ready CLI wrapper code for an MCP server following progressive disclosure patterns
context: fork
agent: wrapper-generator
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
---

# Generate CLI Wrapper Code

This skill generates complete, production-ready CLI wrapper code for MCP servers.

## When This Skill Activates

Use this skill when:
- User wants to create a CLI wrapper for an MCP server
- User requests wrapper code generation
- User needs a Python or TypeScript wrapper
- User asks to wrap MCP tools in a CLI
- User wants to follow progressive disclosure patterns

## What This Skill Does

1. Reads MCP server analysis or configuration
2. Selects appropriate template (Python/TypeScript)
3. Generates complete wrapper code with:
   - PEP 723 header (Python) or package.json (TypeScript)
   - 4-level progressive disclosure help system
   - Function routing and argument parsing
   - Detail level enum support
   - Error handling and validation
   - Output format options (json|text|table)
4. Creates FUNCTION_INFO and FUNCTION_EXAMPLES
5. Tests basic functionality
6. Validates code syntax

## Usage

```
User: "Generate a Python wrapper for the github MCP"
User: "Create a CLI tool for the firecrawl server"
User: "Generate TypeScript wrapper for my calendar API"
```

## Arguments

The skill accepts:
- MCP server name (required)
- Language preference (python|typescript)
- Output path (optional)
- Template style (standard|fastmcp|sdk)

## Output

Generates a complete CLI wrapper file:

```python
#!/usr/bin/env -S uv run
# /// script
# requires-python = ">=3.8"
# dependencies = ["click", "httpx", "rich", "python-dotenv"]
# ///

# Complete implementation with:
# - 4-level progressive disclosure
# - All tool functions
# - Detail level support
# - Error handling
# - Output formatting
```

## Example Output

For GitHub MCP:

```bash
# Generated file: github-cli.py

# Test it
uv run github-cli.py --help          # Level 1: Quick help
uv run github-cli.py list            # Level 2: Function list
uv run github-cli.py info create_issue   # Level 2: Function details
uv run github-cli.py example create_issue  # Level 3: Examples
uv run github-cli.py create_issue --help   # Level 4: Complete reference

# Use it
uv run github-cli.py list_issues --repo=owner/repo
uv run github-cli.py create_issue --title="Bug" --body="Description"
```

## Templates

### Python Standard
- Direct MCP client integration
- Click-based CLI
- Rich formatting
- PEP 723 script

### Python FastMCP
- FastMCP framework
- Decorator-based
- Pydantic validation
- Async support

### TypeScript SDK
- @modelcontextprotocol/sdk
- Zod validation
- Stdio transport
- ESM modules

## Reference Implementations

The skill studies these reference wrappers:
- `/Users/bruno/cli-wrappers/exa.py` - Example structure
- `/Users/bruno/cli-wrappers/github.py` - GitHub wrapper
- `/Users/bruno/cli-wrappers/firecrawl.py` - Firecrawl wrapper
- `/Users/bruno/cli-wrappers/CLAUDE.md` - Development guidelines

## Quality Assurance

Generated code is validated for:
- Correct PEP 723 header syntax
- All dependencies listed
- Executable shebang
- 4-level help completeness
- Function routing correctness
- Error handling coverage
- Output format support
- Syntax correctness

## Agent Context

This skill runs in a forked context using the `wrapper-generator` agent, which specializes in:
- Code generation from templates
- Progressive disclosure implementation
- CLI framework usage (Click, argparse)
- MCP client integration
- Following established patterns from ~/cli-wrappers/
