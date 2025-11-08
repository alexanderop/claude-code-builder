---
description: Convert your .claude directory into a distributable Claude Code plugin
allowed-tools: AskUserQuestion, Bash(ls:*), Bash(mkdir:*), Bash(cp:*), Bash(cat:*), Bash(find:*), Read, Write
---

# Convert .claude Directory to Plugin

You are helping convert an existing `.claude/` directory into a distributable Claude Code plugin.

## Overview

This command helps you take your project-specific customizations (commands, skills, agents, hooks in `.claude/`) and package them into a plugin that can be shared across projects or with your team.

## CRITICAL FIRST STEP: Gather Information

**Before doing anything, use the AskUserQuestion tool to gather:**

1. **Plugin name** (kebab-case, e.g., "my-awesome-plugin")
2. **Plugin description** (clear description of what it does)
3. **Author name** (who is creating this plugin)

**Example AskUserQuestion usage:**
```
Use AskUserQuestion tool with these questions:
- Question 1: "What should the plugin be named?" (header: "Plugin name", note: "Use kebab-case like 'my-plugin'")
- Question 2: "What does this plugin do?" (header: "Description")
- Question 3: "What's the author name?" (header: "Author")
```

Store these values as:
- `PLUGIN_NAME` = the plugin name
- `PLUGIN_DESCRIPTION` = the description
- `AUTHOR_NAME` = the author name

## Workflow Steps

### Step 1: Check Current Directory

First, check if `.claude/` directory exists in the current working directory:

```bash
ls -la .claude/
```

**If .claude directory exists:**
- Proceed to convert it to a plugin
- List what's inside: `ls -R .claude/`

**If .claude directory does NOT exist:**
- Ask user if they want to create an empty plugin structure
- Or if they want to run this from a different directory

### Step 2: Analyze .claude Contents

Check what components exist in `.claude/`:

```bash
# Check for commands
if [ -d ".claude/commands" ]; then
  echo "✅ Commands found"
  ls .claude/commands/
fi

# Check for skills
if [ -d ".claude/skills" ]; then
  echo "✅ Skills found"
  ls .claude/skills/
fi

# Check for agents
if [ -d ".claude/agents" ]; then
  echo "✅ Agents found"
  ls .claude/agents/
fi

# Check for hooks
if [ -d ".claude/hooks" ]; then
  echo "✅ Hooks found"
  ls .claude/hooks/
fi
```

Inform the user what will be included in the plugin.

### Step 3: Create Plugin Directory Structure

Create the plugin directory using the PLUGIN_NAME from Step 1:

```bash
# Create in parent directory to avoid conflicts
cd ..
mkdir <PLUGIN_NAME>
cd <PLUGIN_NAME>
mkdir -p .claude-plugin
```

### Step 4: Copy Components from .claude

Copy the relevant directories from the original `.claude/` directory:

**Important:** Only copy directories that exist!

```bash
# Copy commands if they exist
if [ -d "../[original-project]/.claude/commands" ]; then
  cp -r ../<original-project>/.claude/commands ./
  echo "✅ Copied commands/"
fi

# Copy skills if they exist
if [ -d "../[original-project]/.claude/skills" ]; then
  cp -r ../<original-project>/.claude/skills ./
  echo "✅ Copied skills/"
fi

# Copy agents if they exist
if [ -d "../[original-project]/.claude/agents" ]; then
  cp -r ../<original-project>/.claude/agents ./
  echo "✅ Copied agents/"
fi

# Copy hooks if they exist
if [ -d "../[original-project]/.claude/hooks" ]; then
  cp -r ../<original-project>/.claude/hooks ./
  echo "✅ Copied hooks/"
fi
```

### Step 5: Create Plugin Manifest

Create `plugin.json` with the gathered information:

```bash
cat > .claude-plugin/plugin.json << 'EOF'
{
  "name": "<PLUGIN_NAME>",
  "description": "<PLUGIN_DESCRIPTION>",
  "version": "1.0.0",
  "author": {
    "name": "<AUTHOR_NAME>"
  }
}
EOF
```

**CRITICAL:** Replace `<PLUGIN_NAME>`, `<PLUGIN_DESCRIPTION>`, and `<AUTHOR_NAME>` with actual values from Step 1.

### Step 6: Verify Plugin Structure

Show the user what was created:

```bash
tree <PLUGIN_NAME>
# or if tree is not available:
find <PLUGIN_NAME> -type f
```

Expected structure:
```
<PLUGIN_NAME>/
├── .claude-plugin/
│   └── plugin.json
├── commands/           # If existed in .claude
│   └── [your commands]
├── skills/            # If existed in .claude
│   └── [your skills]
├── agents/            # If existed in .claude
│   └── [your agents]
└── hooks/             # If existed in .claude
    └── [your hooks]
```

### Step 7: Provide Testing Instructions

Give the user these exact testing instructions:

```
✅ Plugin created: <PLUGIN_NAME>

Your .claude/ directory has been converted into a distributable plugin!

📦 What was included:
- [List what was copied from .claude/]

Next Steps:

1. TEST THE PLUGIN (mandatory before distributing):

   # Create test marketplace
   mkdir test-marketplace
   mkdir test-marketplace/.claude-plugin

   # Create marketplace manifest
   cat > test-marketplace/.claude-plugin/marketplace.json << 'EOF'
   {
     "name": "test-marketplace",
     "owner": {"name": "<AUTHOR_NAME>"},
     "plugins": [{
       "name": "<PLUGIN_NAME>",
       "source": "./<PLUGIN_NAME>",
       "description": "<PLUGIN_DESCRIPTION>"
     }]
   }
   EOF

   # Move plugin into marketplace
   mv <PLUGIN_NAME> test-marketplace/

   # In a new Claude session:
   cd ..
   claude
   /plugin marketplace add ./test-marketplace
   /plugin install <PLUGIN_NAME>@test-marketplace
   # Restart Claude and test your commands/skills/agents

2. VERIFY ALL COMPONENTS WORK:
   - [ ] Commands appear in /help
   - [ ] Commands execute correctly
   - [ ] Skills are available
   - [ ] Agents are available
   - [ ] Hooks trigger as expected

3. DISTRIBUTE (only after testing!):
   - Push to git repository
   - Add to a plugin marketplace
   - Share with your team

Your plugin is at: ./<PLUGIN_NAME>
```

## Plugin Structure Rules (CRITICAL)

### ✅ CORRECT Plugin Structure

The plugin should have components at ROOT level:

```
<PLUGIN_NAME>/
├── .claude-plugin/
│   └── plugin.json       # ONLY manifest here
├── commands/             # ✅ At root level
├── skills/               # ✅ At root level
├── agents/               # ✅ At root level
└── hooks/                # ✅ At root level
```

### ❌ WRONG - Common Mistakes

**DO NOT put components inside .claude-plugin/:**
```
<PLUGIN_NAME>/
└── .claude-plugin/
    ├── plugin.json
    ├── commands/         # ❌ WRONG!
    └── skills/           # ❌ WRONG!
```

## Special Cases

### If .claude directory is empty or minimal

Ask the user if they want to:
1. Create an empty plugin structure for future use
2. Cancel and add components first

### If user wants to create from scratch (no .claude)

Guide them to:
1. Create the plugin structure
2. Add their components manually
3. Follow the same testing workflow

## Red Flags to Avoid

**STOP if you notice:**
- Creating files without asking for plugin name first
- Putting components inside `.claude-plugin/` directory
- Using hardcoded "my-plugin" instead of actual gathered name
- Skipping the testing step
- Not copying existing `.claude/` contents

## Summary Template

After successful conversion, provide this summary:

```
✅ Plugin created successfully!

Plugin: <PLUGIN_NAME>
Description: <PLUGIN_DESCRIPTION>
Author: <AUTHOR_NAME>

Converted from .claude/:
- ✅ X commands
- ✅ X skills
- ✅ X agents
- ✅ X hooks

Location: ./<PLUGIN_NAME>/

Next steps:
1. Test using the marketplace instructions above
2. Verify all components work
3. Distribute to your team or community

Remember: Never distribute without testing first!
```

## Key Principles

1. **Ask first** - Get plugin name, description, author before creating files
2. **Convert, don't create** - Copy existing .claude/ contents
3. **Preserve structure** - Components at root, NOT in .claude-plugin/
4. **Test before distributing** - Use local marketplace testing
5. **Never use placeholders** - Always use real values from user
