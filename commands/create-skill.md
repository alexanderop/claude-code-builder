---
description: Generate a new Claude Skill with proper structure and YAML frontmatter using official documentation as reference
argument-hint: [skill-name] [description]
---

# /create-skill

## Purpose
Generate a new Claude Skill with proper structure and YAML frontmatter using official documentation as reference

## Contract
**Inputs:**
- `$1` — SKILL_NAME (lowercase, kebab-case, max 64 characters)
- `$2` — DESCRIPTION (what the skill does and when to use it, max 1024 characters)
- `--personal` — create in ~/.claude/skills/ (default)
- `--project` — create in .claude/skills/
- `--with-examples` — include examples section
- `--with-reference` — include reference documentation file

**Outputs:** `STATUS=<CREATED|EXISTS|FAIL> PATH=<path>`

## Instructions

1. **Validate inputs:**
   - Skill name: lowercase letters, numbers, hyphens only (max 64 chars)
   - Description: non-empty, max 1024 characters
   - Description should include both WHAT the skill does and WHEN to use it

2. **Determine target directory:**
   - Personal (default): `~/.claude/skills/{{SKILL_NAME}}/`
   - Project: `.claude/skills/{{SKILL_NAME}}/`

3. **Check if skill already exists:**
   - If exists: output `STATUS=EXISTS PATH=<path>` and stop

4. **Create skill directory structure:**
   ```
   {{SKILL_NAME}}/
   ├── SKILL.md (required)
   ├── reference.md (if --with-reference)
   └── examples.md (if --with-examples)
   ```

5. **Generate SKILL.md using this template:**
   ```markdown
   ---
   name: {{SKILL_NAME}}
   description: {{DESCRIPTION}}
   ---

   # {{Title Case Skill Name}}

   ## Instructions
   Provide clear, step-by-step guidance for Claude to follow when using this skill.

   1. First step
   2. Second step
   3. Third step

   ## Best practices
   - Keep focused on one capability
   - Be specific and actionable
   - Include error handling guidance

   ## Examples
   Show concrete examples of using this skill.
   ```

6. **Follow official guidelines:**
   - Name: lowercase, numbers, hyphens only (max 64 chars)
   - Description: Include triggers and use cases (max 1024 chars)
   - Instructions: Clear, step-by-step guidance
   - Keep skills focused on single capabilities
   - Make descriptions specific with trigger keywords

7. **Output:**
   - Create directory and files
   - Print: `STATUS=CREATED PATH={{full_path}}`

## Constraints
- Skills are model-invoked (Claude decides when to use them)
- Description must be specific enough for Claude to discover when to use it
- One skill = one capability (stay focused)
- Use forward slashes in all file paths
- Valid YAML syntax required in frontmatter

## Examples

**Basic skill:**
```bash
/create-skill commit-helper "Generate clear git commit messages from diffs. Use when writing commits or reviewing staged changes."
```

**Project skill with examples:**
```bash
/create-skill pdf-processor "Extract text and tables from PDF files. Use when working with PDFs, forms, or document extraction." --project --with-examples
```

**Skill with reference docs:**
```bash
/create-skill api-client "Make REST API calls and handle responses. Use for API testing and integration work." --with-reference
```

## Reference
Based on official Claude Code Agent Skills documentation:
- Personal skills: `~/.claude/skills/`
- Project skills: `.claude/skills/`
- Changes take effect on next Claude Code restart
- Test by asking questions matching the description triggers
