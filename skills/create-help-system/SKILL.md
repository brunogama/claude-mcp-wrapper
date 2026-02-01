---
name: create-help-system
description: Create 4-level progressive disclosure help documentation for CLI wrappers
allowed-tools:
  - Read
  - Write
  - Edit
---

# Create Progressive Disclosure Help System

This skill creates complete 4-level progressive disclosure help documentation for CLI wrappers, achieving 60-70% token savings.

## When This Skill Activates

Use this skill when:
- User needs help documentation for a CLI wrapper
- User wants to implement progressive disclosure
- User asks about 4-level help system
- User needs documentation structure for tools
- Creating or updating CLI wrapper help

## What This Skill Does

1. Analyzes tool functions and their signatures
2. Designs 4-level help hierarchy:
   - Level 1: Quick help (~30 tokens)
   - Level 2: Function list and info (~150 tokens)
   - Level 3: Usage examples (~200 tokens)
   - Level 4: Complete reference (~500 tokens)
3. Generates QUICK_HELP, FUNCTION_INFO, FUNCTION_EXAMPLES
4. Creates CLI commands: list, info, example
5. Adds comprehensive docstrings

## Usage

```
User: "Create help documentation for my CLI wrapper"
User: "Add progressive disclosure to my tool"
User: "Generate 4-level help for these functions"
```

## 4-Level Structure

### Level 1: Quick Help (~30 tokens)
**Command**: `uv run wrapper.py --help`

**Content**:
```
Wrapper CLI - Brief description

Quick Start:
  uv run wrapper.py list         # See all functions
  uv run wrapper.py FUNC --help  # Function help

Common Functions:
  - func1: Brief description
  - func2: Brief description

Environment: API_KEY required
```

**Purpose**: Instant orientation without overwhelming

### Level 2a: Function List (~75 tokens)
**Command**: `uv run wrapper.py list`

**Content**:
```
Available Functions:

• function1: Brief description of what it does
• function2: Brief description of what it does
• function3: Brief description of what it does

For details: uv run wrapper.py info FUNCTION_NAME
```

**Purpose**: Discover available functionality

### Level 2b: Function Info (~150 tokens)
**Command**: `uv run wrapper.py info FUNCTION_NAME`

**Content**:
```
Function: function_name
Signature: function_name(param1: str, param2: int = 10) -> Dict

Description: Detailed explanation of what this function does

Parameters:
┌──────────┬──────┬──────────┬─────────────────┐
│ Name     │ Type │ Required │ Description     │
├──────────┼──────┼──────────┼─────────────────┤
│ param1   │ str  │ Yes      │ What it does    │
│ param2   │ int  │ No (10)  │ What it does    │
└──────────┴──────┴──────────┴─────────────────┘

Returns: Dictionary with keys x, y, z

Related: other_func1, other_func2

For examples: uv run wrapper.py example function_name
```

**Purpose**: Understand specific function details

### Level 3: Examples (~200 tokens)
**Command**: `uv run wrapper.py example FUNCTION_NAME`

**Content**:
```
Usage Examples for function_name

Example 1: Basic usage
$ uv run wrapper.py function_name "value1"
Returns basic result with default parameters

Example 2: With options
$ uv run wrapper.py function_name "value1" --param2=20
Customizes the operation with specific value

Example 3: Summary mode
$ uv run wrapper.py function_name "value1" --detail=summary
Returns minimal data for token efficiency

For complete reference: uv run wrapper.py function_name --help
```

**Purpose**: Copy-paste ready working examples

### Level 4: Complete Reference (~500 tokens)
**Command**: `uv run wrapper.py FUNCTION_NAME --help`

**Content**: Full argparse/click help with:
- Complete parameter documentation
- All options and flags
- Usage syntax
- Error scenarios
- Recovery hints
- Related commands

**Purpose**: Comprehensive reference when needed

## Token Savings

Comparison:

**Traditional Monolithic Help** (all at once):
```
$ tool --help
[500 lines of complete documentation]
Total tokens: ~500
```
Every help invocation costs 500 tokens.

**Progressive Disclosure** (on-demand):
```
$ tool --help
[Quick overview]
Tokens: ~30

$ tool list
[Function list]
Additional tokens: ~75 (cumulative: ~105)

$ tool info function_name
[Function details]
Additional tokens: ~150 (cumulative: ~255)

$ tool example function_name
[Examples]
Additional tokens: ~200 (cumulative: ~455)
```

**Savings**: User typically needs only Level 1 (~30 tokens) = 94% savings
When detailed info needed, progresses to ~255 tokens = 49% savings

## Generated Code

For each tool, creates:

### QUICK_HELP String
```python
QUICK_HELP = """
Wrapper CLI - Brief description

Quick Start:
  uv run wrapper.py list
  uv run wrapper.py info FUNCTION
  uv run wrapper.py FUNCTION --help

Common Functions:
  - func1: description
  - func2: description

Environment: API_KEY required
"""
```

### FUNCTION_INFO Dictionary
```python
FUNCTION_INFO = {
    "function_name": {
        "signature": "function_name(param1: str, param2: int = 10) -> Dict",
        "description": "What this function does in detail",
        "parameters": {
            "param1": {
                "type": "str",
                "required": True,
                "description": "Parameter description"
            },
            "param2": {
                "type": "int",
                "required": False,
                "default": 10,
                "description": "Parameter description"
            }
        },
        "returns": {
            "type": "Dict",
            "description": "What gets returned",
            "structure": {
                "key1": "value description",
                "key2": "value description"
            }
        },
        "detail_levels": ["summary", "normal", "detailed"],
        "related": ["other_func"]
    }
}
```

### FUNCTION_EXAMPLES Dictionary
```python
FUNCTION_EXAMPLES = {
    "function_name": [
        {
            "description": "Basic usage",
            "command": 'uv run wrapper.py function_name "arg1"',
            "explanation": "What this example demonstrates"
        },
        {
            "description": "With options",
            "command": 'uv run wrapper.py function_name "arg1" --opt=val',
            "explanation": "What this shows"
        },
        {
            "description": "Token optimization",
            "command": 'uv run wrapper.py function_name "arg1" --detail=summary',
            "explanation": "Minimal response for efficiency"
        }
    ]
}
```

### CLI Commands
```python
@cli.command()
def list():
    """List all functions (Level 2)."""
    console.print(Panel(
        "\n".join(f"• {name}: {info['description']}"
                  for name, info in FUNCTION_INFO.items()),
        title="Available Functions"
    ))

@cli.command()
@click.argument("function_name")
def info(function_name: str):
    """Show function details (Level 2)."""
    # Format and display FUNCTION_INFO[function_name]

@cli.command()
@click.argument("function_name")
def example(function_name: str):
    """Show usage examples (Level 3)."""
    # Format and display FUNCTION_EXAMPLES[function_name]
```

## Documentation Guidelines

### Quick Help (Level 1)
- Maximum 15 lines
- List top 3-5 most common functions
- Mention environment requirements
- Point to next level

### Function Info (Level 2)
- Complete signature
- All parameters with types
- Return structure
- Related functions
- Point to examples

### Examples (Level 3)
- 2-3 real working examples
- Cover basic, advanced, optimized usage
- Include expected output or behavior
- Copy-paste ready

### Complete Reference (Level 4)
- Generated automatically by argparse/click
- All options documented
- Edge cases covered
- Error scenarios explained

## Output

Creates complete help system code:

```python
# Level 1: Quick Help
QUICK_HELP = """..."""

# Level 2: Function Information
FUNCTION_INFO = {...}

# Level 3: Examples
FUNCTION_EXAMPLES = {...}

# Level 4: Auto-generated by CLI framework
@cli.command()
@click.argument(...)
@click.option(...)
def function_name(...):
    """Complete docstring for Level 4 help."""
    pass
```

## Best Practices

1. **Be concise at Level 1**: Quick orientation only
2. **Be complete at Level 2**: All info without examples
3. **Be practical at Level 3**: Real, working examples
4. **Be comprehensive at Level 4**: Every detail
5. **Cross-reference levels**: Guide users to next level
6. **Update together**: Keep all levels synchronized

## Reference Implementation

Study these examples:
- `/Users/bruno/cli-wrappers/exa.py` - Complete 4-level system
- `/Users/bruno/cli-wrappers/github.py` - GitHub wrapper help
- `/Users/bruno/cli-wrappers/CLAUDE.md` - Documentation standards
