# 👀 Code Review Prompts

Get thorough, actionable code reviews from Claude.

---

## The Standard Review

```
Review this code for:
- Bugs and edge cases
- Performance issues
- Security concerns
- Code style and readability
- Best practices

```[language]
[your code]
```

Context: This is [what the code does] in a [type of project].
```

---

## Focused Reviews

### 🔒 Security Review

```
Security review this code. Look for:
- Injection vulnerabilities (SQL, XSS, etc.)
- Authentication/authorization issues
- Data exposure risks
- Input validation gaps
- Secrets/credentials handling

```[language]
[code]
```

This handles [describe what it does with user data].
```

### ⚡ Performance Review

```
Performance review this code:

```[language]
[code]
```

Context:
- Expected load: [requests/records/users]
- Current observed performance: [metrics if known]
- This runs [how often: per request, batch job, etc.]

Identify bottlenecks and suggest optimizations.
```

### 🏗️ Architecture Review

```
Review the architecture of this [component/module/service]:

Structure:
```
[file tree or description]
```

Key files:
```[language]
[main code snippets]
```

Looking for feedback on:
- Separation of concerns
- Scalability
- Testability
- Maintainability
```

### 📖 Readability Review

```
Review this code for readability and maintainability:

```[language]
[code]
```

Assume a new developer needs to understand and modify this.
- Is the intent clear?
- Are names descriptive?
- Is the complexity justified?
- Would you want to maintain this?
```

---

## PR Review Simulation

```
Act as a senior developer reviewing my PR.

**What changed:** [description]

**Why:** [motivation/ticket/issue]

**Files changed:**

[file1.ts]
```typescript
[code]
```

[file2.ts]
```typescript
[code]
```

Give me:
1. Blocking issues (must fix)
2. Suggestions (should consider)
3. Nitpicks (optional improvements)
4. What's good (positive feedback)
```

---

## Review Checklists

### Before Merging Checklist

```
I'm about to merge this PR. Quick sanity check:

```[language]
[code]
```

Verify:
- [ ] No obvious bugs
- [ ] Error handling is adequate
- [ ] No security red flags
- [ ] Performance is reasonable
- [ ] No leftover debug code
- [ ] Names make sense
```

### Production Readiness

```
Is this code production-ready?

```[language]
[code]
```

Check:
- Error handling and logging
- Edge cases covered
- Resource cleanup (connections, files, etc.)
- Graceful degradation
- Monitoring/observability hooks
```

---

## Comparative Reviews

### "Is This Better?"

```
Which implementation is better and why?

Option A:
```[language]
[code A]
```

Option B:
```[language]
[code B]
```

Context: [what matters - performance, readability, maintainability]
```

### "How Would You Do It?"

```
Here's my implementation:

```[language]
[your code]
```

How would you approach this differently? Show me your version and explain the tradeoffs.
```

---

## Learning from Review

### Understanding Feedback

```
You suggested changing:

From:
```[language]
[original]
```

To:
```[language]
[suggested]
```

Explain:
1. What's wrong with the original?
2. Why is the suggestion better?
3. When might the original be acceptable?
```

### Pattern Recognition

```
Looking at this code:

```[language]
[code with issues]
```

What code smells or anti-patterns do you see? Help me recognize these patterns in future code.
```

---

## Team Review Standards

### Create Review Guidelines

```
Help me create code review guidelines for my team.

Our stack: [technologies]
Team size: [number]
Code review goals: [speed, thoroughness, learning, etc.]

Create a checklist reviewers should follow and standards for PR authors.
```

---

## Pro Tips

1. **Provide context** - What the code does matters for review
2. **Specify concerns** - "I'm worried about X" focuses the review
3. **Include test coverage** - Review should consider what's tested
4. **Mention constraints** - "This needs to be backwards compatible"
5. **Share the why** - Knowing motivation helps judge solutions
