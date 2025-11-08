---
description: Generate CLAUDE.md files for project and key subdirectories
argument-hint: [thoroughness: quick|medium|very-thorough]
---

# /create-md

## Purpose
Analyze project structure using the Explore subagent and generate CLAUDE.md files—one global file describing the project in general, plus nested CLAUDE.md files in important subdirectories for localized context.

## Contract
**Inputs:**
- `$1` — THOROUGHNESS (optional: `quick`, `medium`, `very-thorough`; default: `medium`)

**Outputs:**
- `STATUS=<OK|FAIL>`
- `FILES=<list of created CLAUDE.md paths>`

## Instructions

1. **Explore the project structure** using the Task tool with `subagent_type=Explore`:
   - Understand the overall architecture and technology stack
   - Identify key directories (e.g., `/src`, `/frontend`, `/backend`, `/services`, `/packages`)
   - Find existing setup/build/test commands (package.json, Makefile, etc.)
   - Detect coding conventions (ESLint, Prettier, etc.)
   - Map out the project structure

2. **Create root CLAUDE.md** with:
   - Project overview and architecture map
   - Setup, run, and test commands
   - Code style and conventions
   - Workflow and review expectations
   - Safety guardrails (no secrets, env vars, etc.)
   - Examples of good changes

3. **Create nested CLAUDE.md files** in subdirectories where useful:
   - For monorepo packages/apps with different tech stacks
   - For frontend/backend splits
   - For specialized modules with unique conventions
   - Keep them focused and scoped to that directory's context

4. **Output results**:
   - Print created file paths
   - Print: `STATUS=OK FILES=<comma-separated paths>`

## What to include in CLAUDE.md files

### Root CLAUDE.md
```md
# CLAUDE.md — Project rules

## Overview
One-liner + architecture map.

## Setup & Run
- Install: [command]
- Start dev: [command]
- Tests: [command]

## Code Style
- Linting/formatting tools and configs
- Commit style (Conventional Commits, etc.)

## Safety rails
- Never commit secrets; use .env.example
- Don't modify generated files

## Workflow
- For bugfixes: write failing test → fix → link issue
- For features: create RFC before code

## Examples
- See /docs/examples for typical PRs
```

### Nested CLAUDE.md (e.g., `/frontend/CLAUDE.md`)
```md
# Frontend — Local rules

## Stack
Next.js 14, TypeScript, TailwindCSS

## Key directories
- `/app` — App router pages
- `/components` — Reusable UI components
- `/lib` — Utilities and helpers

## Conventions
- Components: PascalCase, one per file
- Hooks: camelCase, prefix with `use`
- Server components by default; add 'use client' when needed

## Testing
- Run: pnpm test:frontend
- Write tests alongside components (*.test.tsx)
```

## Constraints
- Use the Explore subagent (Task tool) for analysis
- Keep CLAUDE.md files concise and actionable
- Only create nested files where they add value
- Link to existing docs rather than duplicating content

## Example usage
```bash
# Quick exploration and basic CLAUDE.md files
/create-md quick

# Medium depth (default)
/create-md

# Very thorough analysis with comprehensive CLAUDE.md files
/create-md very-thorough
```
