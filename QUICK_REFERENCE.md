# MCP Wrapper Plugin - Quick Reference

## One-Line Summary
Generate efficient CLI wrappers for MCP servers with 60-98.7% token savings through progressive disclosure and code API patterns.

## Quick Commands

```bash
# Analyze MCP server
/claude-mcp-wrapper:analyze <server-name>

# Generate CLI wrapper
/claude-mcp-wrapper:generate <server-name> [python|typescript]

# Create complete project
/claude-mcp-wrapper:scaffold <project-name> [python|typescript]

# Convert traditional code to code API
/claude-mcp-wrapper:convert <input-file>
```

## Example Workflow

```bash
# 1. Analyze
/claude-mcp-wrapper:analyze github

# 2. Generate
/claude-mcp-wrapper:generate github python

# 3. Test
uv run github-cli.py --help
uv run github-cli.py list
uv run github-cli.py info create_issue

# 4. Use
export GITHUB_TOKEN="ghp_..."
uv run github-cli.py list_issues --repo=owner/repo
```

## 4-Level Help System

| Level | Command | Tokens | Content |
|-------|---------|--------|---------|
| 1 | `wrapper.py --help` | ~30 | Quick overview |
| 2a | `wrapper.py list` | ~75 | Function list |
| 2b | `wrapper.py info FUNC` | ~150 | Function details |
| 3 | `wrapper.py example FUNC` | ~200 | Usage examples |
| 4 | `wrapper.py FUNC --help` | ~500 | Complete reference |

Savings: 94% (level 1) vs monolithic help

## Detail Level Enums

| Level | Tokens | Content | Use Case |
|-------|--------|---------|----------|
| SUMMARY | ~50 | Counts only | Quick overview |
| NORMAL | ~200 | Key fields (default) | Most operations |
| DETAILED | ~800 | All fields + relations | Full context needed |
| DEBUG | ~1000 | With metadata | Troubleshooting |

Savings: 97.5% (summary) vs always returning full data

## Token Savings

| Pattern | Before | After | Savings |
|---------|--------|-------|---------|
| Code API orchestration | 150,000 | 2,000 | 98.7% |
| Progressive disclosure | 500 | 30-255 | 49-94% |
| Detail levels (summary) | 2,000 | 50 | 97.5% |
| Deferred loading | 100% | 15% | 85% |
| Programmatic calls | 100% | 63% | 37% |

Source: Anthropic Research, November 2025

## Plugin Components

### Commands (4)
- `analyze`: Analyze MCP server structure
- `generate`: Generate CLI wrapper code
- `scaffold`: Create complete project
- `convert`: Refactor to code API pattern

### Subagents (3)
- `mcp-analyzer` (Blue): Server analysis
- `wrapper-generator` (Green): Code generation
- `schema-optimizer` (Purple): Schema optimization

### Skills (4)
- `analyze-mcp`: Auto-activates on "analyze MCP server"
- `generate-wrapper`: Auto-activates on "generate wrapper"
- `optimize-schema`: Auto-activates on "optimize schema"
- `create-help-system`: Auto-activates on "create help docs"

## Templates

### Python Standard
- PEP 723 script header
- Click for CLI
- Rich for formatting
- Direct MCP client

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

## Testing Generated Wrappers

```bash
# Test help levels
uv run wrapper.py --help
uv run wrapper.py list
uv run wrapper.py info FUNCTION
uv run wrapper.py example FUNCTION
uv run wrapper.py FUNCTION --help

# Test detail levels
uv run wrapper.py FUNCTION --detail=summary
uv run wrapper.py FUNCTION --detail=normal
uv run wrapper.py FUNCTION --detail=detailed
uv run wrapper.py FUNCTION --detail=debug

# Test output formats
uv run wrapper.py FUNCTION --format=json
uv run wrapper.py FUNCTION --format=text
uv run wrapper.py FUNCTION --format=table
```

## Code API Pattern

### Traditional (Token-Heavy)
```python
user = call_tool("get_user", {"id": "123"})
orders = call_tool("get_orders", {"user_id": "123"})
total = call_tool("calculate", {"orders": orders})
# Tokens: ~150,000
```

### Code API (Token-Efficient)
```python
user = api.users.get("123")
orders = api.orders.list(user_id="123")
total = orders.calculate_total()
# Tokens: ~2,000 (98.7% savings)
```

## Environment Setup

```bash
# API credentials
export API_KEY="your_key"
export GITHUB_TOKEN="ghp_..."
export FIRECRAWL_API_KEY="fc-..."

# Optional
export DEBUG=false
export TIMEOUT=30
```

## Reference Implementations

```bash
# Existing CLI wrappers to study
~/cli-wrappers/exa.py          # Complete example
~/cli-wrappers/github.py       # GitHub wrapper
~/cli-wrappers/firecrawl.py    # Firecrawl wrapper
~/cli-wrappers/CLAUDE.md       # Guidelines
```

## Documentation

- **README.md**: Complete usage guide (14 KB)
- **PLUGIN_SUMMARY.md**: Comprehensive summary
- **QUICK_REFERENCE.md**: This file
- Article: `~/.claude/ai_docs/claude/06-WRAPPING-MCPS-CLI-TOOLS.md`

## Troubleshooting

```bash
# Verify plugin loaded
ls -la /Users/bruno/.claude/plugins/claude-mcp-wrapper/.claude-plugin/

# Check commands exist
ls -la /Users/bruno/.claude/plugins/claude-mcp-wrapper/commands/

# Test command availability
/claude-mcp-wrapper:analyze --help
```

## Resources

- MCP Spec: https://modelcontextprotocol.io/
- FastMCP: https://github.com/jlowin/fastmcp
- MCP SDK: https://github.com/modelcontextprotocol/sdk
- Anthropic Research: "Code execution with MCP" (Nov 2025)

## Location

```
/Users/bruno/.claude/plugins/claude-mcp-wrapper/
```

## Version

1.0.0 (2026-02-01)
