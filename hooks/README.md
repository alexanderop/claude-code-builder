# Custom Hooks

This directory contains your custom hooks - automated actions that trigger on specific events.

## What are Hooks?

Hooks are shell commands that execute in response to events like:
- Before/after tool calls
- When prompts are submitted
- When files are created/modified
- When commands complete

## Types of Hooks

### Pre-Tool Hooks
Run before a specific tool is called

### Post-Tool Hooks
Run after a specific tool completes

### Prompt Submit Hooks
Run when you submit a prompt to Claude

### File Change Hooks
Run when files are modified

## Example Hooks

### Example: Auto-format on save

Create a hook that formats code whenever Claude writes a file:

```json
{
  "post-write": "prettier --write $FILE_PATH"
}
```

### Example: Run tests after code changes

```json
{
  "post-edit": "npm test"
}
```

### Example: Lint before commits

```json
{
  "pre-commit": "npm run lint"
}
```

## Configuration

Hooks are typically configured in `.claude/hooks/` or in your Claude Code settings.

Refer to [Claude Code Hooks Documentation](https://docs.claude.com) for detailed configuration.

## Tips

- Keep hooks fast to avoid slowing down your workflow
- Add error handling
- Use hooks for repetitive tasks
- Test hooks thoroughly before relying on them
