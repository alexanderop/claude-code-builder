---
name: creating-claude-plugins
description: Use when creating a Claude Code plugin, before writing any files - ensures correct directory structure (commands at root, not in .claude-plugin), proper markdown format, and mandatory local testing workflow; prevents wrong file placement and validation skipping
---

# Creating Claude Plugins

## Overview

Claude Code plugins extend functionality through custom commands, agents, hooks, skills, and MCP servers. **Critical:** Plugins have a specific directory structure that agents consistently get wrong under pressure.

**Core principle:** Structure first, test locally, distribute after verification.

## When to Create a Plugin

**Create a plugin when:**
- Sharing commands/agents across multiple projects
- Distributing tools to team or community
- Packaging related commands/agents/hooks together
- Need MCP server integration

**Don't create plugin for:**
- Single project customization → Use `.claude/` directory in repo
- One-off commands → Create slash command in project
- Testing ideas → Start in project, migrate to plugin later

## Critical Structure Rules

### ✅ Correct Plugin Structure

```
my-plugin/                    # Plugin root directory
├── .claude-plugin/
│   └── plugin.json          # ONLY manifest here!
├── commands/                 # ✅ At root level
│   └── hello.md             # Markdown, not .sh!
├── agents/                   # ✅ At root level
│   └── helper.md
├── skills/                   # ✅ At root level
│   └── my-skill/
│       └── SKILL.md
└── hooks/                    # ✅ At root level
    └── hooks.json
```

### ❌ WRONG - Common Mistakes

**DO NOT put commands/agents inside .claude-plugin/:**
```
my-plugin/
├── .claude-plugin/
│   ├── plugin.json
│   ├── commands/            # ❌ WRONG LOCATION!
│   └── agents/              # ❌ WRONG LOCATION!
```

**DO NOT use shell scripts for commands:**
```
commands/
└── hello.sh                 # ❌ Wrong! Use .md files
```

**DO NOT confuse installation location with source structure:**
- Plugin source: `my-plugin/commands/`
- Installation destination: `~/.claude/plugins/`
- These are different! Don't mix them.

## Mandatory Workflow

Follow this sequence. No exceptions.

### 1. Check Alternatives First

Before creating plugin, ask:
- Is this specific to one project? → Use `.claude/` in repo
- Testing an idea? → Start with project slash command
- Need to share? → Then create plugin

### 2. Create Directory Structure

```bash
mkdir my-plugin
cd my-plugin
mkdir .claude-plugin commands agents skills hooks
```

### 3. Create Plugin Manifest

```bash
cat > .claude-plugin/plugin.json << 'EOF'
{
  "name": "my-plugin",
  "description": "Clear description of what this plugin does",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  }
}
EOF
```

### 4. Add Components

Commands are markdown prompt files:
```markdown
---
description: What this command does
---

Prompt for Claude explaining what to do when user runs this command.
```

Same pattern for agents and skills.

### 5. Test Locally (MANDATORY)

**You MUST test before claiming "ready" or "done".**

```bash
# Create test marketplace structure
cd ..
mkdir test-marketplace
mkdir test-marketplace/.claude-plugin

# Create marketplace manifest
cat > test-marketplace/.claude-plugin/marketplace.json << 'EOF'
{
  "name": "test-marketplace",
  "owner": {"name": "Test User"},
  "plugins": [{
    "name": "my-plugin",
    "source": "./my-plugin",
    "description": "Testing my plugin"
  }]
}
EOF

# Move your plugin into marketplace
mv my-plugin test-marketplace/

# Start Claude from parent directory
cd ..
claude

# In Claude:
# /plugin marketplace add ./test-marketplace
# /plugin install my-plugin@test-marketplace
# Restart Claude
# Test your commands: /your-command
```

### 6. Verify Before Distribution

**Required verification checklist:**
- [ ] Commands appear in `/help`
- [ ] Commands execute correctly
- [ ] Agents appear in agent selector
- [ ] Hooks trigger as expected
- [ ] No errors during installation

**Only after passing ALL checks:** Ready to distribute.

## Quick Reference

| Component | Location | File Type |
|-----------|----------|-----------|
| Manifest | `.claude-plugin/plugin.json` | JSON |
| Commands | `commands/*.md` | Markdown |
| Agents | `agents/*.md` | Markdown |
| Skills | `skills/*/SKILL.md` | Markdown |
| Hooks | `hooks/hooks.json` | JSON |
| MCP | `.mcp.json` | JSON |

**Key Rules:**
1. Components at plugin root, NOT in `.claude-plugin/`
2. Use `.md` markdown files for commands/agents
3. Test locally with marketplace BEFORE distributing
4. Verify all components work before claiming "ready"

## Common Rationalizations - STOP

| Excuse | Reality |
|--------|---------|
| "Time pressure - skip testing" | Broken plugin wastes more time than 5min testing |
| "User is confident - structure looks right" | User's confidence ≠ correct structure. Verify anyway |
| "Don't waste sunk cost of existing work" | Distributing wrong structure wastes everyone's time |
| "I observed problems but didn't mention them" | If you see problems, STOP and fix before proceeding |
| "Ready for demo/distribution" (without testing) | "Ready" means tested and verified, not "files created" |
| "Just packaging existing work quickly" | Quick ≠ skip validation. Always verify structure |

**All of these mean:** STOP. Follow the mandatory workflow.

## Red Flags - You're About to Fail

- Creating files without checking alternatives first
- Putting components inside `.claude-plugin/` directory
- Using `.sh` or other extensions for commands
- Skipping local marketplace testing
- Claiming "ready" without running verification checklist
- Trusting user's assertion about structure being correct
- Moving fast due to time pressure
- Not questioning confident user's assumptions

**If you notice ANY of these:** STOP. Return to mandatory workflow.

## Documentation

For complete specifications:
- Plugin structure: Claude Code docs on plugins
- Marketplace setup: Plugin marketplaces guide
- Component types: Commands, agents, hooks, MCP docs

## Real-World Impact

**Without this workflow:**
- Plugins with wrong structure don't load
- Commands in wrong locations aren't found
- Team wastes time debugging structure issues
- Distribution before testing = broken installs

**With this workflow:**
- Plugins work first time
- Team can install and use immediately
- Clear structure for maintenance
- Confidence in distribution
