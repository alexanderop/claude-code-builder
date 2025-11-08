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

2. **Create plugin structure:**
   - Create `.claude-plugin/` directory at project root
   - Generate `plugin.json` manifest with proper metadata:
     - name (lowercase, kebab-case)
     - description
     - version (semantic versioning)
     - author information

3. **Organize plugin components:**
   - `commands/` - Slash command markdown files
   - `agents/` - Agent definition markdown files
   - `skills/` - Agent Skills with SKILL.md files
   - `hooks/` - hooks.json for event handlers
   - `.mcp.json` - MCP server configurations (if applicable)

4. **Create marketplace structure for testing:**
   - Set up parent marketplace directory (e.g., `dev-marketplace/`)
   - Create `.claude-plugin/` directory inside the marketplace directory
   - Generate `marketplace.json` with:
     - marketplace name
     - owner information
     - plugins array with source reference to the plugin directory

5. **Generate documentation:**
   - Create/update README.md with:
     - Installation instructions
     - Usage examples
     - Component descriptions
     - Testing guidance

6. **Provide testing workflow:**
   - Local marketplace setup commands
   - Installation verification steps
   - Iteration and debugging guidance

## Reference Documentation

Follow the official Claude Code plugin and marketplace structure:

```
dev-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # Marketplace manifest (REQUIRED for testing)
└── my-plugin/
    ├── .claude-plugin/
    │   └── plugin.json       # Plugin metadata
    ├── commands/             # Custom slash commands (optional)
    │   └── command-name.md
    ├── agents/               # Custom agents (optional)
    │   └── agent-name.md
    ├── skills/               # Agent Skills (optional)
    │   └── skill-name/
    │       └── SKILL.md
    ├── hooks/                # Event handlers (optional)
    │   └── hooks.json
    ├── .mcp.json            # MCP servers (optional)
    └── README.md            # Documentation
```

## Marketplace Manifest Template

The marketplace.json file MUST be created at `<marketplace-dir>/.claude-plugin/marketplace.json`:

```json
{
  "name": "marketplace-name",
  "owner": {
    "name": "Owner Name"
  },
  "plugins": [
    {
      "name": "plugin-name",
      "source": "./plugin-name",
      "description": "Plugin description"
    }
  ]
}
```

## Key Guidelines

- **Plugin manifest**: Use semantic versioning, clear descriptions
- **Marketplace manifest**: MUST create marketplace.json in the parent directory's .claude-plugin/ folder for local testing
- **Commands**: Markdown files with frontmatter (description, argument-hint)
- **Skills**: Create subdirectories with SKILL.md files
- **Testing**: Use local marketplace for iterative development
- **Documentation**: Include installation, usage, and examples

## Constraints
- Must create valid plugin.json schema in plugin's .claude-plugin/ directory
- Must create valid marketplace.json schema in marketplace's .claude-plugin/ directory
- Follow kebab-case naming conventions
- Include proper frontmatter in all markdown files
- Maintain separation between plugin and marketplace manifests
- Plugin directory should be nested inside marketplace directory for local testing
- Output final STATUS line with plugin path

## Example Output
```
Created plugin structure at: ./dev-marketplace/my-plugin
Generated plugin components:
  - .claude-plugin/plugin.json
  - commands/helper.md
  - README.md

Created test marketplace at: ./dev-marketplace
Generated marketplace components:
  - .claude-plugin/marketplace.json

Next steps:
1. cd .. && claude
2. /plugin marketplace add ./dev-marketplace
3. /plugin install my-plugin@dev-marketplace

STATUS=OK PLUGIN_PATH=./dev-marketplace/my-plugin
```
