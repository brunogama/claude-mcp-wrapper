---
description: Generate CLI wrapper code for an MCP server
argument-hint: <mcp-server-name> [python|typescript]
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
---

# Generate CLI Wrapper

Generates production-ready CLI wrapper code for an MCP server following the patterns from your existing CLI wrappers.

## Usage

Creates a complete CLI wrapper with:

1. PEP 723 script header (Python) or package.json (TypeScript)
2. 4-level progressive disclosure help system
3. Function introspection and routing
4. Detail level enums for response optimization
5. Error handling and validation
6. Output format options (json|text|table)

## Process

1. Analyzes MCP server tool definitions
2. Generates wrapper boilerplate based on language choice
3. Creates progressive disclosure help documentation
4. Implements function routing and argument parsing
5. Adds detail level enum support
6. Includes error handling patterns
7. Generates usage examples

## Arguments

- `$ARGUMENTS[0]`: MCP server name (from Claude configuration)
- `$ARGUMENTS[1]`: Language (python|typescript, default: python)
- `--output`: Output file path (default: ./<server-name>.py)
- `--template`: Template style (standard|fastmcp|sdk, default: standard)

## Examples

```bash
# Generate Python wrapper using standard template
/claude-mcp-wrapper:generate github

# Generate TypeScript wrapper
/claude-mcp-wrapper:generate calendar typescript

# Generate FastMCP-based wrapper
/claude-mcp-wrapper:generate database python --template=fastmcp

# Custom output location
/claude-mcp-wrapper:generate myapi python --output=/path/to/myapi-cli.py
```

## Templates

### Standard (Python)
- Direct MCP client integration
- Click-based CLI
- Rich formatting
- PEP 723 compatible

### FastMCP (Python)
- Uses FastMCP framework
- Decorator-based tool definitions
- Built-in validation with Pydantic
- Async support

### SDK (TypeScript)
- Uses @modelcontextprotocol/sdk
- Zod schema validation
- Stdio transport
- ESM module format

## Generated Structure

```python
#!/usr/bin/env -S uv run
# /// script
# requires-python = ">=3.8"
# dependencies = ["click", "httpx", "rich", "python-dotenv"]
# ///

# 4-level progressive disclosure
QUICK_HELP = """..."""
FUNCTION_INFO = {...}
FUNCTION_EXAMPLES = {...}

# CLI commands
@cli.command()
def list(): ...

@cli.command()
def info(function_name): ...

@cli.command()
def example(function_name): ...

# Tool functions with detail level support
@cli.command()
def function_name(args, detail_level="normal"): ...
```

## Next Steps

After generation:
1. Review and customize the generated code
2. Test with `uv run <wrapper>.py --help`
3. Add custom business logic as needed
4. Configure environment variables
