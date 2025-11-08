# Plugin Creator

> Convert your `.claude/` directory into a distributable plugin in seconds.

**The Scenario:** You've built awesome custom commands, skills, or agents in your project's `.claude/` directory. Now you want to:
- Share them with your team
- Use them across multiple projects
- Distribute them to the community
- Package them properly as a plugin

**The Problem:** Converting `.claude/` to a plugin is error-prone:
- Putting components inside `.claude-plugin/` instead of at root level
- Using placeholder names instead of real values
- Wrong file structure that breaks the plugin
- Skipping testing before distribution

**The Solution:** Run `/create-plugin` from your project directory. It detects your `.claude/` contents, asks for plugin info, converts everything to the correct structure, and gives you a ready-to-distribute plugin.

## Why Use This?

### Without `/create-plugin`:
```
❌ Manually copy files from .claude/ to new directory
❌ Guess at the correct plugin structure
❌ Create plugin.json by hand
❌ Risk wrong structure (components in .claude-plugin/)
❌ Forget testing before distribution
❌ Hours of trial and error
```

### With `/create-plugin`:
```
✅ Automatic detection of .claude/ contents
✅ Correct structure every time
✅ Generated plugin.json with your info
✅ Preserves all your commands, skills, agents
✅ Testing instructions included
✅ Ready to distribute in minutes
```

## The Workflow

```bash
# 1. You have a project with custom commands
my-project/
└── .claude/
    └── commands/
        ├── commit-helper.md
        └── review-code.md

# 2. Run the command from your project
cd my-project
claude
> /create-plugin

# 3. Answer 3 questions
Plugin name: git-helpers
Description: Git workflow commands
Author: Your Name

# 4. Get a ready-to-distribute plugin
../git-helpers/
├── .claude-plugin/
│   └── plugin.json
└── commands/
    ├── commit-helper.md
    └── review-code.md
```

## Quick Start

```bash
# 1. Install plugin-creator (one time)
/plugin install plugin-creator@<your-marketplace>

# 2. Go to your project with .claude/ directory
cd my-project

# 3. Run the conversion command
/create-plugin

# 4. Answer 3 questions, get a plugin!
```

## Features

- **Automatic Detection** - Finds your `.claude/` directory and analyzes its contents
- **Smart Conversion** - Copies commands, skills, agents, hooks to correct plugin structure
- **Interactive Setup** - Asks for plugin name, description, and author
- **Correct Structure** - Components at root level (NOT inside `.claude-plugin/`)
- **Generated Manifest** - Creates `plugin.json` with your real information
- **Testing Workflow** - Provides complete marketplace testing instructions
- **Error Prevention** - Ensures correct structure that won't break on installation

## Installation

### Option 1: Install from Local Marketplace (for testing)

1. Create a test marketplace structure:
```bash
# From parent directory of this repo
mkdir test-marketplace
mkdir test-marketplace/.claude-plugin

# Create marketplace manifest
cat > test-marketplace/.claude-plugin/marketplace.json << 'EOF'
{
  "name": "test-marketplace",
  "owner": {"name": "Test User"},
  "plugins": [{
    "name": "plugin-creator",
    "source": "./plugin-creator",
    "description": "Interactive command for creating Claude Code plugins"
  }]
}
EOF

# Move/copy this plugin into marketplace
cp -r plugin-creator test-marketplace/
```

2. Install the plugin:
```bash
# Add the marketplace
/plugin marketplace add ./test-marketplace

# Install the plugin
/plugin install plugin-creator@test-marketplace

# Restart Claude Code
```

### Option 2: Install from Git Repository (when published)

```bash
# This will be available once the plugin is published to a git repository
/plugin install plugin-creator@<marketplace-name>
```

### After Installation

1. **Restart Claude Code** (required for the command to be available)
2. **Verify installation**: Run `/help` and look for `/create-plugin`
3. **Ready to use!** Run `/create-plugin` whenever you need a new plugin

## Usage

### Prerequisites

You need a project with a `.claude/` directory containing customizations:

```
my-project/
└── .claude/
    ├── commands/       # Optional: your slash commands
    ├── skills/         # Optional: your skills
    ├── agents/         # Optional: your agents
    └── hooks/          # Optional: your hooks
```

### Running the Command

Navigate to your project and run:

```bash
cd my-project
claude
> /create-plugin
```

### What Happens Next

The command will:

1. **Detect your `.claude/` directory**
   ```
   ✅ Found .claude/ directory
   ✅ Commands found: 3 files
   ✅ Skills found: 1 directory
   ```

2. **Ask for plugin information**
   - Plugin name (kebab-case like "my-git-helper")
   - Description (what the plugin does)
   - Author name

3. **Convert to plugin structure**
   ```
   Creating ../your-plugin-name/
   ✅ Copied commands/
   ✅ Copied skills/
   ✅ Created plugin.json
   ```

4. **Provide testing instructions** for marketplace installation

### Complete Example Session

```bash
# Starting point: project with .claude/ directory
$ cd my-awesome-project
$ ls .claude/commands/
commit-with-emoji.md  review-pr.md  deploy-staging.md

$ claude
> /create-plugin

Claude: Checking current directory...
✅ Found .claude/ directory
✅ Commands found: 3 files
   - commit-with-emoji.md
   - review-pr.md
   - deploy-staging.md

Claude: I'll help you convert your .claude/ directory into a plugin.
        First, I need some information.

[Interactive questions]
Plugin name: awesome-workflows
Description: Custom git and deployment workflows for my team
Author: Jane Developer

Claude: Converting to plugin structure...
✅ Created ../awesome-workflows/.claude-plugin/
✅ Copied commands/ (3 files)
✅ Generated plugin.json

✅ Plugin created: awesome-workflows

Your .claude/ directory has been converted into a distributable plugin!

📦 What was included:
- ✅ 3 commands

Location: ../awesome-workflows/

Next steps:
1. Test the plugin using local marketplace
2. Verify all commands work
3. Distribute to your team!

[Detailed testing instructions follow...]
```

### What You Get

Your `.claude/` contents packaged as a proper plugin:

```
awesome-workflows/
├── .claude-plugin/
│   └── plugin.json          # ✅ Generated with your info
└── commands/                 # ✅ Copied from .claude/commands
    ├── commit-with-emoji.md
    ├── review-pr.md
    └── deploy-staging.md
```

**plugin.json contents:**
```json
{
  "name": "awesome-workflows",
  "description": "Custom git and deployment workflows for my team",
  "version": "1.0.0",
  "author": {
    "name": "Jane Developer"
  }
}
```

Your customizations, properly packaged. Ready to share.

## Common Mistakes This Prevents

### 🚫 Wrong Directory Structure
**Mistake:** Putting commands inside `.claude-plugin/`
```
❌ WRONG:
my-plugin/
└── .claude-plugin/
    ├── plugin.json
    └── commands/          # ❌ Wrong location!
        └── hello.md
```

**What `/create-plugin` does:**
```
✅ CORRECT:
my-plugin/
├── .claude-plugin/
│   └── plugin.json       # ✅ Only manifest here
└── commands/             # ✅ At root level
    └── hello.md
```

### 🚫 Manual Copy/Paste Errors
**Mistake:** Manually copying files and missing some
```bash
# ❌ Error-prone manual process
mkdir my-plugin
cp .claude/commands/*.md my-plugin/commands/
# Oops, forgot to create commands/ directory first!
# Oops, forgot to copy skills/
```

**What `/create-plugin` does:**
```bash
# ✅ Automatic, complete conversion
Copying commands/ (3 files)
Copying skills/ (1 directory)
Copying agents/ (2 files)
Everything included automatically
```

### 🚫 Forgetting plugin.json
**Mistake:** Creating plugin without proper manifest
```bash
# ❌ No plugin.json or wrong format
{"name": "my-plugin"}  # Missing required fields
```

**What `/create-plugin` does:**
```json
✅ Complete, valid plugin.json
{
  "name": "actual-name-from-user",
  "description": "actual description",
  "version": "1.0.0",
  "author": {"name": "actual author"}
}
```

### 🚫 Skipping Testing
**Mistake:** Distributing plugins without testing them first

**What `/create-plugin` does:**
- ✅ Provides complete testing instructions
- ✅ Shows exact marketplace setup commands
- ✅ Includes verification checklist
- ✅ Ensures plugins work before distribution

## Why Slash Command (Not a Skill)?

**Skills** = Capabilities Claude discovers automatically based on context
**Slash Commands** = Explicit actions you invoke when ready

Creating a plugin is:
- ✅ An intentional, explicit action ("I want to create a plugin now")
- ✅ A linear workflow you start when ready
- ✅ Something you invoke once at the beginning

Therefore, `/create-plugin` as a slash command is the perfect fit!

## This Plugin's Structure

This plugin practices what it preaches - correct structure:

```
plugin-creator/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── commands/                 # ✅ Commands at root level
│   └── create-plugin.md     # The /create-plugin command
├── skills/                   # Reference materials
│   └── creating-claude-plugins/
│       └── SKILL.md
└── README.md
```

## How It Works

The `/create-plugin` command:

1. **Detects `.claude/`** - Checks current directory for existing customizations
2. **Analyzes contents** - Lists what commands, skills, agents, hooks you have
3. **Asks questions** - Uses AskUserQuestion for plugin name, description, and author
4. **Creates structure** - Makes new plugin directory with correct layout
5. **Copies components** - Copies all your `.claude/` contents to the plugin
6. **Generates manifest** - Creates plugin.json with your information
7. **Provides testing** - Gives complete marketplace testing instructions

## FAQ

### Q: What if I don't have a `.claude/` directory?
**A:** The command will detect this and can either:
- Help you create an empty plugin structure, or
- Guide you to add components to `.claude/` first

### Q: Will it modify my original `.claude/` directory?
**A:** No! It copies files to a new plugin directory. Your original `.claude/` remains untouched.

### Q: What if I only have commands, no skills or agents?
**A:** Perfect! The command only copies what exists. If you only have commands, only commands will be in the plugin.

### Q: Can I run this multiple times?
**A:** Yes! Each time you run it, you can create a different plugin or updated version. Great for iteration.

### Q: Where does the plugin get created?
**A:** In the parent directory of your current project. For example:
```
/projects/my-app/           # Your project
/projects/my-app-plugin/    # Created plugin
```

### Q: Do I need to test locally?
**A:** Yes! The command provides complete testing instructions. Always test before distributing to ensure your plugin works correctly.

### Q: Can I share this with my team?
**A:** That's the whole point! Once tested, you can:
- Push to git and add to a marketplace
- Share the directory directly
- Distribute however you like

### Q: What if my `.claude/` has CLAUDE.md or other files?
**A:** The command focuses on distributable components (commands, skills, agents, hooks). Project-specific files like CLAUDE.md stay in your project.

## Development & Contributing

### Local Testing
The `.claude/` directory contains a local copy for testing without installation. To test:

```bash
cd /path/to/plugin-creator
claude
# The command will be available locally
/create-plugin
```

### Contributing
Found a bug or have an improvement? Contributions welcome!

## Why This Plugin Exists

**The Real-World Scenario:**

You build awesome slash commands for your project. They're in `.claude/commands/`. They work great. Your teammate sees them and says "I want those too!"

Now what?

You could:
1. Tell them to copy the files manually (they'll mess it up)
2. Document the process (they won't read it)
3. Create a plugin manually (you'll spend 2 hours getting the structure right)

Or you could run `/create-plugin` and be done in 2 minutes.

**This plugin exists because:**
- Developers create great `.claude/` customizations all the time
- Those customizations are valuable to others
- Converting to a plugin manually is error-prone and frustrating
- One command should do the whole thing correctly

We automated the "detect, ask, convert, test" workflow so you can focus on building great tools instead of debugging plugin structure issues.

## License

MIT License - feel free to use, modify, and distribute

## Author

**Alexander Opalic**

Created to save future developers from the pain of debugging incorrectly structured plugins.
