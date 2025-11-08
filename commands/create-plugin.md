---
description: Convert a project into a Claude Code plugin
argument-hint: [project-path]
---

# /create-plugin

## Purpose
Guide users through converting an existing project into a properly structured Claude Code plugin, following official documentation standards.

## Contract
**Inputs:** `[project-path]` — Optional path to project directory (defaults to current directory)
**Outputs:** `STATUS=<OK|FAIL> PLUGIN_PATH=<path>`

## Instructions

1. **Analyze the project structure:**
   - Identify existing components that could become plugin features
   - Check for slash commands, agents, skills, hooks, or MCP integrations
   - Review documentation files

2. **Create plugin and marketplace structure:**
   - Create `.claude-plugin/` directory at project root
   - Generate `plugin.json` manifest with proper metadata:
     - name (lowercase, kebab-case)
     - description
     - version (semantic versioning)
     - author information (object with name and optional url)
     - repository (string URL, not object)
     - license (optional)
   - Generate `marketplace.json` in the same directory with:
     - marketplace name
     - owner information
     - plugins array with source reference `"./"` (self-reference)

3. **Organize plugin components:**
   - `commands/` - Slash command markdown files
   - `agents/` - Agent definition markdown files
   - `skills/` - Agent Skills with SKILL.md files
   - `hooks/` - hooks.json for event handlers
   - `.mcp.json` - MCP server configurations (if applicable)

4. **Generate documentation:**
   - Create/update README.md with:
     - Installation instructions
     - Usage examples
     - Component descriptions
     - Testing guidance

5. **Provide testing workflow:**
   - Local marketplace setup commands
   - Installation verification steps
   - Iteration and debugging guidance

## Reference Documentation

Follow the official Claude Code plugin and marketplace structure:

```
my-plugin/
├── .claude-plugin/
│   ├── marketplace.json      # Marketplace manifest
│   └── plugin.json          # Plugin metadata
├── commands/                 # Custom slash commands (optional)
│   └── command-name.md
├── agents/                   # Custom agents (optional)
│   └── agent-name.md
├── skills/                   # Agent Skills (optional)
│   └── skill-name/
│       └── SKILL.md
├── hooks/                    # Event handlers (optional)
│   └── hooks.json
├── .mcp.json                # MCP servers (optional)
└── README.md                # Documentation
```

## Plugin Manifest Template

The plugin.json file MUST be created at `<plugin-dir>/.claude-plugin/plugin.json`:

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Plugin description",
  "author": {
    "name": "Author Name",
    "url": "https://github.com/username"
  },
  "repository": "https://github.com/username/plugin-name",
  "license": "MIT"
}
```

**Important:** The `repository` field must be a string URL, not an object. Using an object format like `{"type": "git", "url": "..."}` will cause validation errors.

## Marketplace Manifest Template

The marketplace.json file MUST be created at `<plugin-dir>/.claude-plugin/marketplace.json` alongside plugin.json:

```json
{
  "name": "marketplace-name",
  "owner": {
    "name": "Owner Name"
  },
  "plugins": [
    {
      "name": "plugin-name",
      "source": "./",
      "description": "Plugin description"
    }
  ]
}
```

## Key Guidelines

- **Plugin manifest**: Use semantic versioning, clear descriptions
- **Marketplace manifest**: MUST create marketplace.json in the same .claude-plugin/ directory alongside plugin.json
- **Commands**: Markdown files with frontmatter (description, argument-hint)
- **Skills**: Create subdirectories with SKILL.md files
- **Testing**: Use local marketplace for iterative development
- **Documentation**: Include installation, usage, and examples

## Constraints
- Must create valid plugin.json schema in .claude-plugin/ directory
- Must create valid marketplace.json schema in the same .claude-plugin/ directory
- The repository field in plugin.json MUST be a string URL, not an object
- Follow kebab-case naming conventions
- Include proper frontmatter in all markdown files
- Marketplace source must reference "./" to point to the plugin directory itself
- Output final STATUS line with plugin path

## Example Output
```
Created plugin structure at: ./my-plugin
Generated components:
  - .claude-plugin/plugin.json
  - .claude-plugin/marketplace.json
  - commands/helper.md
  - README.md

Next steps:
1. cd my-plugin && claude
2. /plugin marketplace add .
3. /plugin install my-plugin@my-plugin-dev

Or for GitHub-based installation:
1. Push to GitHub repository
2. /plugin marketplace add username/my-plugin
3. /plugin install my-plugin@my-plugin-dev

STATUS=OK PLUGIN_PATH=./my-plugin
```
