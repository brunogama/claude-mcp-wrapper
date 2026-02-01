---
name: optimize-schema
description: Optimize MCP tool schemas for maximum token efficiency using detail level enums and response filtering
context: fork
agent: schema-optimizer
allowed-tools:
  - Read
  - Write
  - Edit
---

# Optimize Tool Schemas

This skill optimizes MCP tool schemas to minimize token usage while maximizing functionality.

## When This Skill Activates

Use this skill when:
- User wants to optimize tool schemas for token efficiency
- User asks about reducing token usage in MCP tools
- User needs detail level recommendations
- User wants to improve response structures
- User requests schema enhancement with examples

## What This Skill Does

1. Analyzes current tool schemas
2. Identifies token waste opportunities
3. Designs optimal detail level enums
4. Defines response structures for each level
5. Adds schema examples
6. Estimates token savings
7. Generates optimized implementation code

## Usage

```
User: "Optimize the schema for get_users tool"
User: "How can I reduce tokens in my MCP responses?"
User: "Add detail levels to my tool schemas"
User: "Optimize token usage for the analytics API"
```

## What Gets Optimized

### Before Optimization
```json
{
  "name": "get_users",
  "description": "Get all users",
  "inputSchema": {
    "type": "object",
    "properties": {
      "status": {"type": "string"}
    }
  }
}
```

Always returns full data = wasteful.

### After Optimization
```json
{
  "name": "get_users",
  "description": "Get users with filters. Use detail_level to control response size.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "status": {
        "type": "string",
        "enum": ["active", "inactive"],
        "example": "active"
      },
      "detail_level": {
        "type": "string",
        "enum": ["summary", "normal", "detailed", "debug"],
        "default": "normal",
        "description": "summary=counts, normal=key fields, detailed=all fields, debug=with metadata"
      }
    }
  },
  "examples": [
    {
      "input": {"status": "active", "detail_level": "summary"},
      "output": {"count": 150}
    }
  ]
}
```

## Detail Level Definitions

### SUMMARY (~50 tokens)
Minimal information:
- Counts and aggregations only
- No individual records
- Use for quick overview

### NORMAL (~200 tokens)
Balanced response (default):
- Key fields only
- Essential information
- Best for most operations

### DETAILED (~800 tokens)
Complete information:
- All fields
- Related resources
- Use when full context needed

### DEBUG (~1000 tokens)
Diagnostic information:
- Complete data
- Query metadata
- Performance metrics
- Use for troubleshooting

## Token Savings Examples

| Tool | Before | After (Summary) | Savings |
|------|--------|----------------|---------|
| get_users | 2000 | 50 | 97.5% |
| list_orders | 1500 | 80 | 94.7% |
| analytics | 3000 | 120 | 96.0% |

## Implementation Code

For each optimized tool, generates:

```python
from enum import Enum
from fastmcp import FastMCP

app = FastMCP()

class DetailLevel(str, Enum):
    SUMMARY = "summary"
    NORMAL = "normal"
    DETAILED = "detailed"
    DEBUG = "debug"

@app.tool()
async def get_users(
    status: str = None,
    detail_level: DetailLevel = DetailLevel.NORMAL
) -> dict:
    """Get users with optional filters."""
    users = db.query_users(status=status)

    if detail_level == DetailLevel.SUMMARY:
        return {"count": len(users)}
    elif detail_level == DetailLevel.NORMAL:
        return {
            "users": [
                {"id": u.id, "name": u.name}
                for u in users
            ]
        }
    # ... etc
```

## Output Report

Generates optimization report:

```markdown
# Schema Optimization Report: get_users

## Current Schema
[Original schema]

## Token Analysis
- Current average: 2000 tokens
- Optimization potential: 97.5%

## Optimized Schema
[Enhanced schema with detail levels]

## Detail Levels
- SUMMARY: 50 tokens (97.5% savings)
- NORMAL: 200 tokens (90% savings)
- DETAILED: 800 tokens (60% savings)
- DEBUG: 1000 tokens (50% savings)

## Implementation
[Optimized code]

## Testing Commands
```bash
uv run tool.py get_users --detail=summary
uv run tool.py get_users --detail=normal
```
```

## Best Practices Applied

1. **Summary first**: Always design minimal response
2. **Normal as default**: Balanced for most use cases
3. **Examples included**: Schema has usage examples
4. **Token estimates**: Document expected sizes
5. **Progressive enhancement**: Each level adds value

## Agent Context

This skill runs in a forked context using the `schema-optimizer` agent, which specializes in:
- Token efficiency analysis
- Detail level enum design
- Response structure optimization
- Schema enhancement
- MCP best practices from Anthropic research
