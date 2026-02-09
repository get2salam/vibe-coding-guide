# ♻️ Refactoring Prompts

Transform messy code into clean, maintainable solutions.

---

## The Refactoring Request

```
Refactor this code to be more [goal]:

```[language]
[code to refactor]
```

Constraints:
- Keep the same external behavior
- [Any other constraints]

Explain your changes.
```

---

## Refactoring Goals

### 🧹 Clean Up Messy Code

```
This code works but it's a mess. Clean it up:

```[language]
[messy code]
```

Make it:
- More readable
- Better organized
- Following [language] conventions

Don't change functionality.
```

### 📦 Extract and Modularize

```
This function/file is doing too much:

```[language]
[large code block]
```

Break it into smaller, focused [functions/modules/classes].
Show me the refactored structure.
```

### 🔄 Replace Pattern

```
I'm using this pattern throughout my codebase:

```[language]
[current pattern example]
```

I want to replace it with [new pattern/approach].

Show me:
1. The new pattern
2. A migration example
3. Things to watch out for
```

### 🏛️ Apply Design Pattern

```
This code could benefit from a design pattern:

```[language]
[code]
```

It needs to handle [requirements that suggest a pattern].

What pattern fits here? Show me the refactored version.
```

### 🔀 Modernize Syntax

```
Update this code to use modern [language] features:

```[language]
[older style code]
```

Target version: [version]
Apply: [specific features like async/await, optional chaining, etc.]
```

---

## Specific Refactors

### Reduce Nesting

```
This code has too many levels of nesting:

```[language]
[deeply nested code]
```

Flatten it using:
- Early returns
- Guard clauses
- Extraction

Keep logic identical.
```

### Simplify Conditionals

```
Simplify this complex conditional logic:

```[language]
[code with complex if/else]
```

Make it more readable while preserving behavior.
```

### DRY It Up

```
This code has repetition:

```[language]
[code with duplication]
```

Extract the common pattern into a reusable [function/component/utility].
```

### Add Types

```
Add TypeScript types to this JavaScript code:

```javascript
[untyped code]
```

Infer appropriate types. Use strict typing, avoid 'any'.
```

### Performance Refactor

```
Refactor for better performance:

```[language]
[inefficient code]
```

Currently: [performance issue]
Target: [performance goal]

Optimize without changing the API/interface.
```

---

## Safe Refactoring

### Refactor with Tests

```
I want to refactor this but I'm scared of breaking it:

```[language]
[code]
```

First, write tests that capture the current behavior.
Then, do the refactor.
Finally, show me the tests still pass.
```

### Incremental Refactoring

```
This is too big to refactor all at once:

```[language]
[large codebase section]
```

Create a step-by-step refactoring plan:
1. Each step should be a small, safe change
2. Tests should pass after each step
3. Show me the first 3 steps in detail
```

### Strangler Fig Pattern

```
I need to gradually replace this legacy code:

Old implementation:
```[language]
[legacy code]
```

New approach I want: [describe]

Help me implement the strangler fig pattern - wrap the old, gradually replace with new.
```

---

## Before and After

### Explain the Improvement

```
You refactored my code from:
```[language]
[before]
```

To:
```[language]
[after]
```

Explain each change and why it's an improvement.
```

### Measure the Impact

```
For this refactoring:

Before:
```[language]
[original]
```

After:
```[language]
[refactored]
```

Quantify improvements:
- Lines of code
- Cyclomatic complexity
- Readability (subjective 1-10)
- Testability
```

---

## Refactoring Principles

### When to Refactor

Good times to refactor:
- Before adding a new feature to an area
- After getting something working ("make it work, make it right")
- When you have good test coverage
- During code review

### When NOT to Refactor

- Right before a deadline
- Without understanding what the code does
- Without tests (write them first!)
- Just for the sake of it (if it ain't broke...)

---

## Pro Tips

1. **Test first** - Always have tests before refactoring
2. **Small steps** - Big refactors = big risks
3. **One thing at a time** - Separate behavioral changes from structural
4. **Commit often** - Easy rollback if something breaks
5. **Keep it working** - Code should pass tests after each change
