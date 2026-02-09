# 📝 Documentation Prompts

Generate clear, useful documentation effortlessly.

---

## The Documentation Request

```
Document this code:

```[language]
[code]
```

Include:
- What it does (overview)
- How to use it (examples)
- Parameters/options
- Return values
- Edge cases/gotchas
```

---

## Documentation Types

### 📖 README Generation

```
Generate a README for my project:

Project: [name]
Purpose: [what it does]
Stack: [technologies]
Key features:
- [feature 1]
- [feature 2]

Include:
- Badges
- Installation
- Quick start
- Configuration
- API overview
- Contributing section
```

### 📚 API Documentation

```
Document this API endpoint:

```[language]
[route handler code]
```

Include:
- Endpoint description
- Request format (method, headers, body)
- Response format (success and error)
- Example curl/fetch
- Authentication requirements
```

### 💬 Code Comments

```
Add clear comments to this code:

```[language]
[code without comments]
```

Add:
- Function/class docstrings
- Inline comments for complex logic
- TODO markers where appropriate

Follow [language] documentation conventions.
```

### 📋 JSDoc/TSDoc/Docstrings

```
Add comprehensive JSDoc to these functions:

```javascript
[functions without docs]
```

Include:
- @description
- @param with types
- @returns
- @throws
- @example
```

---

## Specific Documentation Tasks

### Document Architecture

```
Document the architecture of my project:

Structure:
```
[file tree]
```

Key components:
- [Component 1]: [brief role]
- [Component 2]: [brief role]

Create an ARCHITECTURE.md explaining:
- High-level overview
- Component interactions
- Data flow
- Key design decisions
```

### Create Runbook

```
Create a runbook for [system/service]:

Common operations needed:
- [Operation 1]
- [Operation 2]

Include:
- Step-by-step procedures
- Troubleshooting guides
- Emergency contacts/escalation
- Monitoring dashboard links
```

### Write Tutorial

```
Write a tutorial for [feature/workflow]:

Target audience: [beginner/intermediate/advanced] developers
Goal: By the end, reader can [outcome]

Structure:
1. Introduction (why this matters)
2. Prerequisites
3. Step-by-step guide
4. Common pitfalls
5. Next steps
```

### Changelog Entry

```
Write a changelog entry for these changes:

Changes made:
- [Change 1]
- [Change 2]

Version: [X.Y.Z]
Type: [major/minor/patch]

Follow Keep a Changelog format.
```

---

## Documentation from Code

### Generate API Reference

```
Generate API reference documentation from this code:

```[language]
[module/class/API code]
```

Format as markdown with:
- Table of contents
- Each function/method documented
- Type signatures
- Examples for each
```

### Extract Inline Docs

```
Extract documentation from these comments:

```[language]
[code with comments]
```

Create structured markdown documentation.
```

### Reverse Engineer Docs

```
This code has no documentation. Create docs by analyzing it:

```[language]
[undocumented code]
```

Figure out:
- What it does
- How to use it
- Expected inputs/outputs
- Dependencies
```

---

## Documentation Improvement

### Review and Improve

```
Review this documentation:

```markdown
[existing docs]
```

For the code:
```[language]
[code being documented]
```

Identify:
- Missing information
- Inaccuracies
- Unclear explanations
- Outdated content
```

### Simplify Technical Docs

```
Simplify this documentation for [audience]:

```markdown
[complex documentation]
```

Make it:
- Easier to scan
- Less jargon-heavy
- More example-driven
- Actionable
```

### Add Examples

```
These docs are missing examples:

```markdown
[documentation without examples]
```

Add practical examples for each section that show real-world usage.
```

---

## Documentation Standards

### Create Style Guide

```
Create documentation standards for my team:

Tech stack: [technologies]
Doc tools: [what we use for docs]

Define:
- Writing style
- Structure templates
- Required sections
- Example formats
- Review process
```

### README Template

```
Create a README template for our organization's projects:

Standard sections we need:
- [list required sections]

Our conventions:
- [any specific requirements]

Make it copy-paste ready with placeholder text.
```

---

## Pro Tips

1. **Examples > Descriptions** - Show, don't just tell
2. **Keep it updated** - Outdated docs are worse than no docs
3. **Link liberally** - Connect related concepts
4. **Test your docs** - Follow your own instructions
5. **Know your audience** - Adjust complexity accordingly
6. **Document the why** - Not just what, but why it's designed this way
