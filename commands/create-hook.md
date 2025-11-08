---
description: Create and configure Claude Code hooks with reference documentation
argument-hint: [hook-type] [matcher] [command]
---

# /create-hook

## Purpose
Create and configure Claude Code hooks with reference documentation and interactive guidance.

## Contract
**Inputs:**
- `$1` — HOOK_TYPE (optional: PreToolUse, PostToolUse, UserPromptSubmit, Notification, Stop, SubagentStop, PreCompact, SessionStart, SessionEnd)
- `$2` — MATCHER (optional: tool name pattern, e.g., "Bash", "Edit|Write", "*")
- `$3` — COMMAND (optional: shell command to execute)

**Outputs:** `STATUS=<OK|FAIL> HOOK_FILE=<path>`

## Instructions

1. **Determine hook configuration mode:**
   - If no arguments provided: Show interactive menu of hook types with examples
   - If HOOK_TYPE provided: Guide user through creating that specific hook
   - If all arguments provided: Create hook directly

2. **Validate inputs:**
   - HOOK_TYPE must be one of the valid hook events
   - MATCHER should be a valid tool name or pattern
   - COMMAND should be a valid shell command

3. **Reference documentation:**
   Review the Claude Code hooks documentation for best practices and examples:

   **Hook Events:**
   - `PreToolUse`: Runs before tool calls (can block them)
   - `PostToolUse`: Runs after tool calls complete
   - `UserPromptSubmit`: Runs when user submits a prompt
   - `Notification`: Runs when Claude Code sends notifications
   - `Stop`: Runs when Claude Code finishes responding
   - `SubagentStop`: Runs when subagent tasks complete
   - `PreCompact`: Runs before compact operations
   - `SessionStart`: Runs when session starts/resumes
   - `SessionEnd`: Runs when session ends

4. **Common use cases and examples:**

   **Logging Bash commands:**
   ```json
   {
     "hooks": {
       "PreToolUse": [{
         "matcher": "Bash",
         "hooks": [{
           "type": "command",
           "command": "jq -r '\"\\(.tool_input.command) - \\(.tool_input.description // \"No description\")\"' >> ~/.claude/bash-command-log.txt"
         }]
       }]
     }
   }
   ```

   **Auto-format TypeScript files:**
   ```json
   {
     "hooks": {
       "PostToolUse": [{
         "matcher": "Edit|Write",
         "hooks": [{
           "type": "command",
           "command": "jq -r '.tool_input.file_path' | { read file_path; if echo \"$file_path\" | grep -q '\\.ts$'; then npx prettier --write \"$file_path\"; fi; }"
         }]
       }]
     }
   }
   ```

   **Block sensitive file edits:**
   ```json
   {
     "hooks": {
       "PreToolUse": [{
         "matcher": "Edit|Write",
         "hooks": [{
           "type": "command",
           "command": "python3 -c \"import json, sys; data=json.load(sys.stdin); path=data.get('tool_input',{}).get('file_path',''); sys.exit(2 if any(p in path for p in ['.env', 'package-lock.json', '.git/']) else 0)\""
         }]
       }]
     }
   }
   ```

   **Desktop notifications:**
   ```json
   {
     "hooks": {
       "Notification": [{
         "matcher": "",
         "hooks": [{
           "type": "command",
           "command": "notify-send 'Claude Code' 'Awaiting your input'"
         }]
       }]
     }
   }
   ```

5. **Security considerations:**
   - Hooks run automatically with your environment's credentials
   - Always review hook implementation before registering
   - Be cautious with hooks that execute external commands
   - Avoid hardcoding sensitive data in hook commands
   - Use exit code 2 to block operations in PreToolUse hooks

6. **Hook creation workflow:**
   - Determine appropriate hook event for the use case
   - Define matcher pattern (specific tool or "*" for all)
   - Write shell command that processes JSON input via stdin
   - Test hook command independently before registering
   - Choose storage location (user vs project settings)
   - Register hook using `/hooks` command or manual JSON edit

7. **Debugging hooks:**
   - Check hook configuration in settings files
   - Verify hook command works standalone with test JSON
   - Review Claude Code output for hook execution errors
   - Use `jq` to inspect JSON data passed to hooks
   - Check exit codes (0=continue, 2=block in PreToolUse)

8. **Output:**
   - If interactive mode: Guide user through hook creation
   - If direct mode: Show the JSON configuration to add
   - Provide instructions for registering the hook
   - Print: `STATUS=OK HOOK_FILE=~/.claude/settings.json` or `STATUS=OK HOOK_FILE=.claude/settings.local.json`

## Constraints
- Hooks must be valid shell commands
- JSON structure must follow hooks schema
- PreToolUse hooks can block operations with exit code 2
- Hooks should be idempotent and handle errors gracefully
- Consider performance impact of hooks in tight loops

## Examples

**Interactive mode:**
```bash
/create-hook
# Shows menu of hook types and common use cases
```

**Guided mode:**
```bash
/create-hook PreToolUse
# Guides through creating a PreToolUse hook
```

**Direct mode:**
```bash
/create-hook PreToolUse "Bash" "jq -r '.tool_input.command' >> ~/.claude/commands.log"
# Creates hook configuration directly
```

## Reference
For complete documentation, see: https://docs.claude.com/en/hooks
