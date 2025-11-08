# Custom Subagents

This directory contains your custom subagents - specialized AI agents for specific tasks in your workflow.

## What are Subagents?

Subagents are specialized versions of Claude that are configured for specific tasks like:
- Code review with your specific standards
- Testing with your preferred frameworks
- Documentation in your preferred style
- Deployment with your specific process

## How to Create a Subagent

Create a markdown file that describes the agent's purpose and instructions:

### Example: `code-reviewer.md`

```markdown
---
description: Reviews code according to my personal standards
---

# Code Review Agent

You are a code review agent specialized in reviewing code according to Alexander's coding standards.

## Review Checklist

1. **Code Style**
   - Check naming conventions
   - Verify formatting
   - Ensure consistency

2. **Best Practices**
   - Look for security issues
   - Check for performance problems
   - Verify error handling

3. **Testing**
   - Ensure test coverage
   - Check test quality
   - Verify edge cases

## Output Format

Provide feedback in this format:
- Issues (critical fixes needed)
- Suggestions (improvements to consider)
- Praise (what was done well)
```

### Example: `documentation-writer.md`

```markdown
---
description: Writes documentation in my preferred style
---

# Documentation Writer Agent

You write documentation following Alexander's documentation standards.

## Documentation Style

- Start with a clear purpose statement
- Include practical examples
- Add troubleshooting sections
- Keep it concise but complete

## Documentation Structure

1. Overview
2. Quick Start
3. Detailed Usage
4. API Reference
5. Troubleshooting
6. FAQ
```

## Using Subagents

Once created, you can invoke these agents in your commands or have Claude automatically use them for relevant tasks.

## Tips

- Define clear responsibilities for each agent
- Include specific instructions and checklists
- Provide examples of expected output
- Update as your processes evolve
