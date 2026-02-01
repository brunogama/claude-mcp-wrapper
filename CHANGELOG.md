# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2026-02-01

### Added

- Initial release of claude-mcp-wrapper plugin
- **Commands**
  - `/claude-mcp-wrapper:analyze` - Analyze MCP server structure and generate wrapping strategy
  - `/claude-mcp-wrapper:generate` - Generate CLI wrapper code (Python/TypeScript)
  - `/claude-mcp-wrapper:scaffold` - Create complete project structure for new wrapper
  - `/claude-mcp-wrapper:convert` - Convert traditional tool calls to code API patterns
- **Subagents**
  - `mcp-analyzer` - Analyzes MCP servers, designs namespaces, plans progressive disclosure
  - `wrapper-generator` - Generates production-ready CLI wrapper code
  - `schema-optimizer` - Optimizes tool schemas for token efficiency
- **Skills**
  - `analyze-mcp` - MCP server analysis skill
  - `generate-wrapper` - Wrapper code generation skill
  - `optimize-schema` - Schema optimization skill
  - `create-help-system` - Progressive disclosure documentation skill
- **Documentation**
  - README.md with complete usage guide
  - PLUGIN_SUMMARY.md with comprehensive summary
  - QUICK_REFERENCE.md for quick command lookup

### Token Efficiency Features

- Progressive disclosure (4-level help): 60-70% token savings
- Detail level enums (summary/normal/detailed/debug): 90-97% savings
- Code API orchestration patterns: up to 98.7% savings
- Based on Anthropic's November 2025 MCP research

[Unreleased]: https://github.com/USER/claude-mcp-wrapper/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/USER/claude-mcp-wrapper/releases/tag/v1.0.0
