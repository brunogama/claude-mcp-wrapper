---
description: Create complete project structure for a new MCP wrapper
argument-hint: <project-name> [python|typescript]
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
---

# Scaffold MCP Wrapper Project

Creates a complete, production-ready project structure for an MCP server wrapper with all necessary files and configuration.

## Usage

Scaffolds a new wrapper project with:

1. Project directory structure
2. CLI wrapper script
3. Configuration files (package.json or pyproject.toml)
4. Environment template (.env.example)
5. Documentation (README.md)
6. Testing setup
7. CI/CD configuration (optional)

## Process

1. Creates project directory structure
2. Generates CLI wrapper code
3. Creates progressive disclosure help documentation
4. Sets up dependency management
5. Creates environment configuration template
6. Generates comprehensive README
7. Adds testing framework setup
8. Optionally configures CI/CD

## Arguments

- `$ARGUMENTS[0]`: Project name (will create directory)
- `$ARGUMENTS[1]`: Language (python|typescript, default: python)
- `--mcp-server`: MCP server name to wrap (optional, can configure later)
- `--with-tests`: Include testing setup (default: true)
- `--with-ci`: Include CI/CD configuration (default: false)
- `--output-dir`: Parent directory for project (default: current directory)

## Examples

```bash
# Create Python wrapper project
/claude-mcp-wrapper:scaffold my-api-cli

# Create TypeScript wrapper project with CI
/claude-mcp-wrapper:scaffold calendar-cli typescript --with-ci

# Scaffold for specific MCP server
/claude-mcp-wrapper:scaffold github-cli python --mcp-server=github

# Custom output location
/claude-mcp-wrapper:scaffold api-tools --output-dir=/Users/bruno/projects
```

## Generated Structure (Python)

```
my-api-cli/
├── cli.py                    # Main CLI script (PEP 723)
├── .env.example              # Environment template
├── README.md                 # Full documentation
├── CLAUDE.md                 # Development guidelines
├── pyproject.toml            # Optional: for development
├── tests/
│   ├── test_cli.py
│   └── test_integration.py
├── .gitignore
└── .github/
    └── workflows/
        └── test.yml          # CI/CD (if --with-ci)
```

## Generated Structure (TypeScript)

```
my-api-cli/
├── src/
│   ├── index.ts              # Main entry point
│   ├── cli.ts                # CLI implementation
│   └── mcp-client.ts         # MCP integration
├── package.json
├── tsconfig.json
├── .env.example
├── README.md
├── tests/
│   └── cli.test.ts
├── .gitignore
└── .github/
    └── workflows/
        └── test.yml
```

## Generated Files

### CLI Script
Complete wrapper with 4-level help system, all commands, and tool routing.

### README.md
Comprehensive documentation including:
- Installation instructions
- 4-level help system overview
- Usage examples for all functions
- Environment variable configuration
- Troubleshooting guide

### CLAUDE.md
Development guidelines following your existing patterns:
- Code quality rules
- 4-level progressive disclosure patterns
- Testing requirements
- Commit message format
- Security guidelines

### .env.example
Template for required environment variables:
```bash
# API Configuration
API_KEY=your_api_key_here
API_URL=https://api.example.com

# Optional
DEBUG=false
TIMEOUT=30
```

### Tests
Testing setup with examples:
- Unit tests for CLI commands
- Integration tests for MCP server communication
- Mocked responses for offline testing

## Post-Scaffold Steps

After scaffolding:

```bash
# Navigate to project
cd my-api-cli

# Install dependencies (Python)
uv run cli.py --help

# Install dependencies (TypeScript)
npm install

# Configure environment
cp .env.example .env
# Edit .env with your credentials

# Test the wrapper
uv run cli.py list           # Python
npm start -- list            # TypeScript

# Run tests
pytest tests/                # Python
npm test                     # TypeScript
```

## Next Steps

1. Configure environment variables in `.env`
2. Test basic functionality: `uv run cli.py list`
3. Customize tool implementations as needed
4. Add project-specific documentation
5. Initialize git repository: `git init && git add . && git commit -m "Initial scaffold"`
