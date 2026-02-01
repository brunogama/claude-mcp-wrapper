# MCP Wrapper Plugin

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](CHANGELOG.md)

A Claude Code plugin specialized in wrapping MCP servers into efficient CLI tools following progressive disclosure and code API patterns.

**Version**: 1.0.0
**Author**: Bruno
**License**: MIT
**Based on**: Anthropic's "Code execution with MCP" article (November 2025)

## Overview

This plugin helps you create production-ready CLI wrappers for MCP servers that achieve up to 98.7% token reduction through:

1. **Progressive Disclosure**: 4-level help system (60-70% token savings)
2. **Code API Patterns**: Orchestration over individual calls (98.7% savings)
3. **Detail Level Enums**: Flexible response filtering (90-97% savings)
4. **Optimal Schemas**: Enhanced tool definitions with examples

## Installation

The plugin is located at:
```
/Users/bruno/.claude/plugins/claude-mcp-wrapper/
```

To enable it in Claude Code, it should auto-load from the plugins directory. If not:

```bash
# Verify plugin structure
ls -la /Users/bruno/.claude/plugins/claude-mcp-wrapper/

# Check plugin.json
cat /Users/bruno/.claude/plugins/claude-mcp-wrapper/.claude-plugin/plugin.json
```

## Commands

### /claude-mcp-wrapper:analyze

Analyzes an MCP server and generates a wrapping strategy.

**Usage**:
```bash
/claude-mcp-wrapper:analyze <mcp-server-name> [json|markdown|summary]
```

**Examples**:
```bash
/claude-mcp-wrapper:analyze github
/claude-mcp-wrapper:analyze firecrawl json
/claude-mcp-wrapper:analyze ref summary
```

**Output**:
- Tool inventory grouped by namespace
- Progressive disclosure structure (4 levels)
- Detail level recommendations
- Token efficiency estimates
- Implementation roadmap

### /claude-mcp-wrapper:generate

Generates production-ready CLI wrapper code.

**Usage**:
```bash
/claude-mcp-wrapper:generate <mcp-server-name> [python|typescript]
```

**Options**:
- `--output`: Output file path
- `--template`: Template style (standard|fastmcp|sdk)

**Examples**:
```bash
/claude-mcp-wrapper:generate github
/claude-mcp-wrapper:generate calendar typescript
/claude-mcp-wrapper:generate database python --template=fastmcp
/claude-mcp-wrapper:generate myapi --output=/path/to/cli.py
```

**Output**:
Complete CLI wrapper with:
- PEP 723 header (Python) or package.json (TypeScript)
- 4-level progressive disclosure
- All tool functions
- Detail level support
- Error handling
- Output formats (json|text|table)

### /claude-mcp-wrapper:scaffold

Creates complete project structure for a new MCP wrapper.

**Usage**:
```bash
/claude-mcp-wrapper:scaffold <project-name> [python|typescript]
```

**Options**:
- `--mcp-server`: MCP server to wrap
- `--with-tests`: Include testing setup (default: true)
- `--with-ci`: Include CI/CD config (default: false)
- `--output-dir`: Parent directory

**Examples**:
```bash
/claude-mcp-wrapper:scaffold my-api-cli
/claude-mcp-wrapper:scaffold calendar-cli typescript --with-ci
/claude-mcp-wrapper:scaffold github-cli --mcp-server=github
```

**Output**:
Complete project with:
- CLI wrapper script
- Documentation (README.md, CLAUDE.md)
- Environment template (.env.example)
- Testing setup
- CI/CD configuration (optional)

### /claude-mcp-wrapper:convert

Converts traditional tool calls to code API patterns.

**Usage**:
```bash
/claude-mcp-wrapper:convert <input-file>
```

**Options**:
- `--output`: Output file path
- `--estimate-only`: Show savings without refactoring
- `--preserve-comments`: Keep original code as comments

**Examples**:
```bash
/claude-mcp-wrapper:convert workflow.py
/claude-mcp-wrapper:convert script.py --estimate-only
/claude-mcp-wrapper:convert legacy.py --preserve-comments
```

**Output**:
- Refactored code using code API patterns
- Token analysis (before/after/savings)
- Pattern summary
- Migration guide

## Subagents

### mcp-analyzer

Analyzes MCP server structures and generates wrapping strategies.

**Specializes in**:
- MCP server introspection
- Tool categorization
- Namespace design
- Progressive disclosure planning
- Token efficiency analysis

**Tools**: Read, Bash, Grep, Glob
**Permission Mode**: acceptEdits
**Color**: Blue

### wrapper-generator

Generates production-ready CLI wrapper code.

**Specializes in**:
- Code generation from templates
- Progressive disclosure implementation
- CLI framework integration (Click, argparse)
- MCP client setup
- Following established patterns

**Tools**: Read, Write, Edit, Bash, Glob
**Permission Mode**: acceptEdits
**Color**: Green

### schema-optimizer

Optimizes tool schemas for maximum token efficiency.

**Specializes in**:
- Token efficiency analysis
- Detail level enum design
- Response structure optimization
- Schema enhancement
- Adding examples to schemas

**Tools**: Read, Write, Edit
**Permission Mode**: acceptEdits
**Color**: Purple

## Skills

### analyze-mcp

Analyzes MCP server structure and creates wrapping strategy.

**Triggers when**:
- User asks about analyzing an MCP server
- User wants to understand MCP capabilities
- User needs wrapping strategy
- Preparing to create CLI wrapper

**Uses**: mcp-analyzer agent (forked context)

### generate-wrapper

Generates production-ready CLI wrapper code.

**Triggers when**:
- User wants to create CLI wrapper
- User requests wrapper code generation
- User needs Python or TypeScript wrapper
- User asks to wrap MCP tools

**Uses**: wrapper-generator agent (forked context)

### optimize-schema

Optimizes tool schemas for token efficiency.

**Triggers when**:
- User wants to optimize schemas
- User asks about reducing token usage
- User needs detail level recommendations
- User wants to improve response structures

**Uses**: schema-optimizer agent (forked context)

### create-help-system

Creates 4-level progressive disclosure help documentation.

**Triggers when**:
- User needs help documentation
- User wants progressive disclosure
- User asks about 4-level help
- Creating/updating CLI wrapper help

**Tools**: Read, Write, Edit

## Key Patterns

### 1. Progressive Disclosure (4 Levels)

**Level 1: Quick Help** (~30 tokens)
```bash
uv run wrapper.py --help
```
Quick overview, common functions, environment requirements.

**Level 2a: Function List** (~75 tokens)
```bash
uv run wrapper.py list
```
All available functions with brief descriptions.

**Level 2b: Function Info** (~150 tokens)
```bash
uv run wrapper.py info FUNCTION_NAME
```
Detailed function signature, parameters, returns, related functions.

**Level 3: Examples** (~200 tokens)
```bash
uv run wrapper.py example FUNCTION_NAME
```
2-3 working examples with explanations.

**Level 4: Complete Reference** (~500 tokens)
```bash
uv run wrapper.py FUNCTION_NAME --help
```
Full documentation with all options, edge cases, error scenarios.

**Savings**: 60-70% vs monolithic help (30 tokens vs 500 tokens typical)

### 2. Detail Level Enums

**SUMMARY** (~50 tokens):
```python
{"count": 150, "by_status": {...}}
```
Minimal information, counts only.

**NORMAL** (~200 tokens):
```python
{"users": [{"id": "1", "name": "John"}], "count": 10}
```
Key fields only (default).

**DETAILED** (~800 tokens):
```python
{"users": [full_objects], "relationships": {...}}
```
All fields plus relationships.

**DEBUG** (~1000 tokens):
```python
{...data, "query_time_ms": 45, "metadata": {...}}
```
Complete data with diagnostics.

**Savings**: 90-97% (summary) vs always returning full data

### 3. Code API Pattern

**Traditional** (Token-Heavy):
```python
user = call_tool("get_user", {"id": "123"})
orders = call_tool("get_orders", {"user_id": "123"})
total = call_tool("calculate", {"orders": orders})
```
Each call reloads context. Tokens: ~150,000

**Code API** (Token-Efficient):
```python
user = api.users.get("123")
orders = api.orders.list(user_id="123")
total = orders.calculate_total()
```
Single execution context. Tokens: ~2,000

**Savings**: 98.7% (per Anthropic research)

### 4. Namespacing

```python
# Good: Clear boundaries
api.users.get(id)
api.users.update(id, data)
api.orders.list(filters)
api.orders.create(data)

# Avoid: Naming collisions
get_user(id)
update_user(id, data)
list_orders(filters)
create_order(data)
```

## Token Savings Reference

Based on Anthropic's November 2025 research:

| Pattern | Before | After | Savings |
|---------|--------|-------|---------|
| Code API orchestration | 150,000 | 2,000 | 98.7% |
| Progressive disclosure | 500 | 30-255 | 49-94% |
| Detail levels (summary) | 2,000 | 50 | 97.5% |
| Deferred tool loading | 100% | 15% | 85% |
| Programmatic calling | 100% | 63% | 37% |

## Workflow Examples

### Complete Wrapper Creation

```bash
# 1. Analyze MCP server
/claude-mcp-wrapper:analyze github

# 2. Generate wrapper code
/claude-mcp-wrapper:generate github python

# 3. Test the wrapper
uv run github-cli.py --help
uv run github-cli.py list
uv run github-cli.py info create_issue

# 4. Use it
export GITHUB_TOKEN="ghp_..."
uv run github-cli.py list_issues --repo=owner/repo
```

### Full Project Scaffold

```bash
# Create complete project
/claude-mcp-wrapper:scaffold my-api-cli python --with-tests

# Navigate and configure
cd my-api-cli
cp .env.example .env
# Edit .env with credentials

# Test
uv run cli.py list
pytest tests/
```

### Code Refactoring

```bash
# Convert traditional calls to code API
/claude-mcp-wrapper:convert legacy-workflow.py

# Review savings estimate
/claude-mcp-wrapper:convert script.py --estimate-only

# Apply with preserved original
/claude-mcp-wrapper:convert old.py --preserve-comments --output=new.py
```

### Schema Optimization

```bash
# Ask Claude to optimize
"Optimize the schema for my get_users tool to reduce token usage"

# The optimize-schema skill will activate and:
# 1. Analyze current schema
# 2. Add detail_level parameter
# 3. Define response structures
# 4. Generate optimized code
# 5. Estimate token savings
```

## Reference Implementations

Study these existing wrappers as templates:

```bash
# Python wrappers with 4-level help
~/cli-wrappers/exa.py
~/cli-wrappers/github.py
~/cli-wrappers/firecrawl.py
~/cli-wrappers/ref.py

# Development guidelines
~/cli-wrappers/CLAUDE.md
```

## Environment Variables

Generated wrappers typically need:

```bash
# API credentials
export API_KEY="your_api_key_here"

# MCP server configuration (if not in ~/.config/claude/mcp.json)
export MCP_SERVER_URL="http://localhost:3000"

# Optional settings
export DEBUG=false
export TIMEOUT=30
```

## Testing Generated Wrappers

```bash
# Verify basic functionality
uv run wrapper.py --help          # Should show quick help
uv run wrapper.py list            # Should list functions
uv run wrapper.py info FUNCTION   # Should show function details
uv run wrapper.py example FUNCTION # Should show examples

# Test a function
uv run wrapper.py FUNCTION_NAME args...

# Test detail levels
uv run wrapper.py FUNCTION --detail=summary
uv run wrapper.py FUNCTION --detail=normal
uv run wrapper.py FUNCTION --detail=detailed

# Test output formats
uv run wrapper.py FUNCTION --format=json
uv run wrapper.py FUNCTION --format=text
uv run wrapper.py FUNCTION --format=table
```

## Troubleshooting

### Plugin Not Loading

Check plugin structure:
```bash
ls -la /Users/bruno/.claude/plugins/claude-mcp-wrapper/.claude-plugin/
cat /Users/bruno/.claude/plugins/claude-mcp-wrapper/.claude-plugin/plugin.json
```

### Command Not Found

Verify command files exist:
```bash
ls -la /Users/bruno/.claude/plugins/claude-mcp-wrapper/commands/
```

Commands should be accessible as:
```bash
/claude-mcp-wrapper:analyze
/claude-mcp-wrapper:generate
/claude-mcp-wrapper:scaffold
/claude-mcp-wrapper:convert
```

### Subagent Not Activating

Check agent definitions:
```bash
ls -la /Users/bruno/.claude/plugins/claude-mcp-wrapper/agents/
```

Subagents activate automatically when:
- Skills use `context: fork`
- Commands delegate to agents
- User explicitly requests analysis/generation/optimization

### Skill Not Triggering

Skills should auto-activate when user query matches description. Check:
```bash
ls -la /Users/bruno/.claude/plugins/claude-mcp-wrapper/skills/*/SKILL.md
```

Manual activation:
```
User: "Analyze the github MCP server"  -> activates analyze-mcp skill
User: "Generate wrapper for firecrawl" -> activates generate-wrapper skill
User: "Optimize my tool schemas"       -> activates optimize-schema skill
```

## Development

### Adding New Templates

Edit wrapper-generator agent:
```bash
/Users/bruno/.claude/plugins/claude-mcp-wrapper/agents/wrapper-generator.md
```

Add template code in Step 3 of the Generation Process section.

### Customizing Help Structure

Edit create-help-system skill:
```bash
/Users/bruno/.claude/plugins/claude-mcp-wrapper/skills/create-help-system/SKILL.md
```

Modify the 4-level structure definitions.

### Adding New Commands

Create new command file:
```bash
/Users/bruno/.claude/plugins/claude-mcp-wrapper/commands/my-command.md
```

Follow the format of existing commands with YAML frontmatter.

## Resources

### Documentation
- Anthropic: "Code execution with MCP" (Nov 2025)
- Anthropic: "Writing effective tools for AI agents" (Sep 2025)
- Anthropic: "Advanced tool use" (Nov 2025)
- MCP Specification: https://modelcontextprotocol.io/

### Frameworks
- FastMCP: https://github.com/jlowin/fastmcp
- MCP SDK: https://github.com/modelcontextprotocol/sdk
- MCP Servers: https://github.com/modelcontextprotocol/servers

### Local References
- Article: /Users/bruno/.claude/ai_docs/claude/06-WRAPPING-MCPS-CLI-TOOLS.md
- CLI Wrappers: ~/cli-wrappers/
- Guidelines: ~/cli-wrappers/CLAUDE.md

## Contributing

Contributions are welcome. Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes following the existing code style
4. Update documentation as needed
5. Update CHANGELOG.md with your changes
6. Commit your changes (`git commit -m 'feat: add amazing feature'`)
7. Push to the branch (`git push origin feature/amazing-feature`)
8. Open a Pull Request

### Commit Message Format

Use [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` new feature
- `fix:` bug fix
- `docs:` documentation changes
- `refactor:` code refactoring
- `test:` adding tests
- `chore:` maintenance tasks

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For issues or questions:
1. Check this README
2. Review reference implementations in ~/cli-wrappers/
3. Read the comprehensive manual at /Users/bruno/.claude/ai_docs/claude/06-WRAPPING-MCPS-CLI-TOOLS.md
4. Examine existing wrappers as examples

## Version History

**1.0.0** (2026-02-01)
- Initial release
- 4 commands: analyze, generate, scaffold, convert
- 3 subagents: mcp-analyzer, wrapper-generator, schema-optimizer
- 4 skills: analyze-mcp, generate-wrapper, optimize-schema, create-help-system
- Based on Anthropic's November 2025 MCP research
- Token savings: 60-98.7% across various patterns
