# Custom Coding Rules

This directory contains your personal coding rules, patterns, and conventions that Claude should follow when working with you.

## How to Add Rules

Create markdown files in this directory that describe your coding preferences:

### Example: `typescript-style.md`

```markdown
# TypeScript Code Style

## Naming Conventions
- Use camelCase for variables and functions
- Use PascalCase for classes and interfaces
- Use UPPER_CASE for constants

## File Organization
- One component per file
- Index files for barrel exports
- Test files alongside source files

## Type Safety
- Prefer interfaces over types for objects
- Always define return types for functions
- Use strict null checks
```

### Example: `project-structure.md`

```markdown
# Project Structure Rules

## Directory Layout
- `/src` for source code
- `/tests` for test files
- `/docs` for documentation
- `/scripts` for build scripts

## File Naming
- Use kebab-case for files
- Use .spec.ts for test files
```

## How Claude Uses These Rules

When you install this plugin, Claude will have access to these rules and can reference them when:
- Writing new code
- Refactoring existing code
- Reviewing code
- Making architectural decisions

## Tips

- Be specific and concrete
- Include examples
- Update as your preferences evolve
- Group related rules together
