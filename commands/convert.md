---
description: Convert traditional tool calls to code API patterns
argument-hint: <input-file>
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
---

# Convert to Code API Pattern

Converts traditional MCP tool calls to code API patterns for dramatic token efficiency improvements.

## Usage

Analyzes code that makes traditional tool calls and refactors it to use code API orchestration patterns, achieving up to 98.7% token reduction.

## Traditional vs Code API

### Traditional Approach (Token-Heavy)
```python
# Multiple individual tool calls
user = call_tool("get_user", {"id": "123"})
orders = call_tool("get_orders", {"user_id": "123"})
total = call_tool("calculate_total", {"orders": orders})
result = call_tool("update_summary", {"user_id": "123", "total": total})
```

Token usage: ~150,000 tokens (each call reloads context)

### Code API Approach (Token-Efficient)
```python
# Code orchestration
user = api.users.get("123")
orders = api.orders.list(user_id="123")
total = orders.calculate_total()
api.users.update_summary("123", total=total)
```

Token usage: ~2,000 tokens (single execution context)

## Process

1. Parses input code or MCP conversation logs
2. Identifies repeated tool call patterns
3. Groups related operations
4. Refactors to code API pattern
5. Adds detail level parameters where beneficial
6. Optimizes intermediate result handling
7. Generates refactored code with token savings estimate

## Arguments

- `$ARGUMENTS[0]`: Input file path (Python/TypeScript code or conversation log)
- `--output`: Output file for refactored code (default: <input>-refactored.py)
- `--estimate-only`: Only show token savings estimate without refactoring
- `--preserve-comments`: Keep original code as comments

## Examples

```bash
# Convert Python code to code API pattern
/claude-mcp-wrapper:convert workflow.py

# Estimate savings without changing code
/claude-mcp-wrapper:convert script.py --estimate-only

# Convert with preserved original code
/claude-mcp-wrapper:convert legacy.py --preserve-comments --output=modern.py

# Convert from conversation log
/claude-mcp-wrapper:convert conversation.json
```

## Conversion Patterns

### Pattern 1: Sequential Tool Calls
```python
# Before (Traditional)
user = get_user(id="123")
update_user(id="123", name="John")
log_action(action="updated", user_id="123")

# After (Code API)
user = api.user.get("123")
user.name = "John"
api.user.update(user)
api.log.record("updated", user.id)
```

### Pattern 2: Batch Operations
```python
# Before (Traditional)
users = get_users(status="pending")
for user in users:
    update_status(user.id, "active")
    send_notification(user.email)

# After (Code API)
users = api.users.list(status="pending")
api.users.batch_update(
    [u.id for u in users],
    status="active",
    notify=True
)
```

### Pattern 3: Progressive Detail Levels
```python
# Before (Traditional)
users = get_users_full_detail()  # Returns everything
count = len(users)

# After (Code API)
users = api.users.list(detail="summary")  # Just count
count = users.count
```

### Pattern 4: State Management
```python
# Before (Traditional)
tx = create_transaction()
add_to_transaction(tx.id, operation1)
add_to_transaction(tx.id, operation2)
commit_transaction(tx.id)

# After (Code API)
with api.transaction() as tx:
    tx.add(operation1)
    tx.add(operation2)
    # Auto-commits on exit
```

## Output Report

The conversion generates:

1. **Refactored Code**: Optimized code using API patterns
2. **Token Analysis**:
   - Before: estimated tokens
   - After: estimated tokens
   - Savings: percentage and absolute reduction
3. **Pattern Summary**: Which patterns were applied
4. **Recommendations**: Further optimization opportunities
5. **Migration Guide**: Steps to adopt the new code

## Token Savings Examples

Based on Anthropic's research:

| Operation Type | Traditional | Code API | Savings |
|---------------|-------------|----------|---------|
| Multi-step workflow | 150,000 | 2,000 | 98.7% |
| Batch operations | 80,000 | 5,000 | 93.8% |
| Query with filtering | 45,000 | 3,000 | 93.3% |
| State management | 120,000 | 8,000 | 93.3% |

## Best Practices

After conversion:

1. **Test thoroughly**: Code API pattern changes execution flow
2. **Add error handling**: Wrap operations in try/except blocks
3. **Use detail levels**: Start with "summary", drill down as needed
4. **Batch when possible**: Group related operations
5. **State management**: Use context managers for transactions

## Next Steps

After conversion:
1. Review the refactored code
2. Test with your MCP server
3. Measure actual token usage
4. Iterate on detail level parameters
5. Consider wrapping in CLI tool with `/claude-mcp-wrapper:generate`
