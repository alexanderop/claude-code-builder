# Project Templates

This directory contains your project templates and boilerplate for common project types.

## What are Templates?

Templates are starter configurations for the types of projects you frequently create:
- Frontend applications
- Backend services
- CLI tools
- Libraries
- Full-stack applications

## How to Create Templates

Create directories with the structure and files you want for each project type:

### Example: Node.js API Template

```
templates/
└── nodejs-api/
    ├── package.json
    ├── tsconfig.json
    ├── src/
    │   ├── index.ts
    │   ├── routes/
    │   └── models/
    ├── tests/
    └── README.md
```

### Example: React Component Template

```
templates/
└── react-component/
    ├── Component.tsx
    ├── Component.test.tsx
    ├── Component.module.css
    └── index.ts
```

## Using Templates

You can create commands that scaffold new projects using these templates:

```markdown
# /new-api command

Create a new Node.js API project using my template:

1. Copy template from `templates/nodejs-api/`
2. Replace placeholders with project name
3. Install dependencies
4. Initialize git repository
```

## Template Variables

Use placeholders in your templates that can be replaced:

- `{{PROJECT_NAME}}` - The project name
- `{{DESCRIPTION}}` - Project description
- `{{AUTHOR}}` - Your name
- `{{DATE}}` - Current date

## Tips

- Keep templates up-to-date with your current best practices
- Include comprehensive README files in templates
- Add useful npm scripts or build configurations
- Document any setup steps needed after scaffolding
