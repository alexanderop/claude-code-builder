# Das Hirn

> My personal Claude Code extension - custom rules, subagents, and workflows

This is my personal coding brain - a Claude Code plugin that contains all my custom configurations, rules, subagents, and workflows that make coding with Claude exactly how I want it.

## What's Inside

This plugin is a living collection of:

- **Custom Commands** - Slash commands tailored to my workflow
- **Custom Rules** - Code style, patterns, and conventions I follow
- **Custom Subagents** - Specialized agents for specific tasks
- **Hooks** - Automated actions that trigger on events
- **Templates** - Boilerplate for my common project types

## Structure

```
das-hirn/
├── .claude-plugin/
│   └── plugin.json         # Plugin manifest
├── commands/               # Custom slash commands
│   └── create-plugin.md   # Plugin creator tool
├── .claude/               # Local development configs
│   └── commands/          # Commands for developing this plugin
└── README.md
```

## Installation

### For Personal Use

```bash
# Clone or copy this plugin
cd ~/your-projects

# Add to Claude Code via marketplace or local installation
/plugin marketplace add ./das-hirn
/plugin install das-hirn@local
```

### Adding Custom Components

As you develop your own rules and subagents:

1. **Add Commands**: Create `.md` files in `commands/`
2. **Add Rules**: Document in a `rules/` directory
3. **Add Subagents**: Configure in `agents/` directory
4. **Add Hooks**: Set up in `hooks/` directory

## Development

This plugin includes the `/create-plugin` command which helps convert `.claude/` directories into distributable plugins - useful for creating additional plugins from your work.

## Philosophy

This plugin evolves with my coding style. It's not meant to be shared as-is, but rather to be a personal extension that grows with every project.

### Current Focus Areas

- [x] Plugin creation tooling
- [ ] Personal code style rules
- [ ] Custom subagents for common tasks
- [ ] Project templates
- [ ] Workflow automation hooks

## Version History

- **1.0.0** - Initial transformation from plugin-creator to personal coding brain

## Author

**Alexander Opalic**

This is my brain extension for Claude Code.
