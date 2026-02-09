# 🏚️ Legacy Codebase Workflow

Working with existing codebases using vibe coding.

---

## Phase 1: Understanding

### Initial Exploration

```
I'm working with a legacy codebase. Help me understand it.

Overview:
- Age: [how old]
- Stack: [technologies, if known]
- Size: [rough LOC or file count]
- Documentation: [exists / sparse / none]

Here's the file structure:
```
[paste tree output]
```

Give me a high-level understanding of the architecture.
```

### Deep Dive into Area

```
I need to understand this part of the codebase:

```[language]
[code from the area]
```

Explain:
- What does this code do?
- How does it fit into the larger system?
- What are the key abstractions?
- Any obvious issues or tech debt?
```

### Map Dependencies

```
Help me map the dependencies in this module:

```[language]
[code]
```

Show me:
- What this depends on
- What depends on this
- Critical coupling points
- Potential for isolation
```

---

## Phase 2: Safe Changes

### Add Tests First

```
I need to modify this legacy code but there are no tests:

```[language]
[code to be changed]
```

Write characterization tests that capture current behavior.
I'll verify they pass before making changes.
```

### Small, Safe Refactor

```
Refactor this legacy code safely:

```[language]
[code]
```

Rules:
- No behavioral changes
- Small steps
- Each step should be independently verifiable

Show me the step-by-step plan.
```

### Strangler Fig Pattern

```
I want to gradually replace this legacy system:

Current:
```[language]
[legacy code]
```

New approach: [what you want]

Help me implement strangler fig:
1. Wrap the legacy code
2. Route new functionality through wrapper
3. Gradually replace internals
```

---

## Phase 3: Adding Features

### Feature in Legacy Context

```
Add [FEATURE] to this legacy codebase.

Existing patterns I should follow:
```[language]
[example of existing pattern]
```

Constraints:
- Must integrate with existing [X]
- Cannot change [Y]
- Should be consistent with codebase style

Build the feature following existing conventions.
```

### Minimal Viable Change

```
I need to add [capability] with minimal changes.

Current code:
```[language]
[relevant code]
```

Requirements:
- [What it needs to do]

Find the smallest change that achieves this.
Touch as few files as possible.
```

---

## Phase 4: Modernization

### Identify Tech Debt

```
Review this code for tech debt:

```[language]
[code]
```

Categorize issues:
- 🔴 Critical (security, data loss risk)
- 🟡 Important (performance, maintainability)
- 🟢 Nice to fix (style, minor improvements)

Prioritize what to fix first.
```

### Incremental Modernization

```
Modernize this code incrementally:

```[language]
[older style code]
```

Current: [old patterns/versions]
Target: [modern patterns/versions]

Create a migration plan that:
1. Can be done in small PRs
2. Doesn't require big-bang deployment
3. Has rollback strategy at each step
```

### Dependency Updates

```
This project has outdated dependencies:

```json
[package.json or equivalent]
```

Create an update plan:
- Which updates are safe?
- Which need careful testing?
- Which might have breaking changes?
- What order should I update them?
```

---

## Phase 5: Documentation

### Reverse Engineer Docs

```
This code has no documentation:

```[language]
[undocumented code]
```

Create documentation by analyzing the code:
- What it does
- How to use it
- Dependencies
- Configuration options
- Common gotchas
```

### Document Tribal Knowledge

```
I've figured out how this system works through trial and error.

The system: [description]

What I've learned:
- [Insight 1]
- [Insight 2]
- [Gotcha 1]

Help me document this for future developers.
```

---

## Anti-Patterns to Avoid

### ❌ Big Bang Rewrite

Don't:
```
Rewrite this entire legacy system in [modern framework]
```

### ✅ Instead

```
Identify one module to modernize first. It should be:
- Isolated
- Well-understood
- High-value to improve

Start there.
```

### ❌ Changing Everything at Once

Don't:
```
Refactor this and also add the new feature and update the dependencies
```

### ✅ Instead

```
Let's do this in order:
1. First, add tests
2. Then, refactor
3. Then, add the feature

One concern per PR.
```

---

## Survival Tips

1. **Assume there's a reason** - Weird code often exists for a reason
2. **Test before touching** - Characterization tests save lives
3. **Small commits** - Easy to bisect and revert
4. **Document discoveries** - Save the next person time
5. **Don't gold-plate** - Fix what needs fixing, not everything
6. **Communicate** - Legacy changes often have stakeholders
