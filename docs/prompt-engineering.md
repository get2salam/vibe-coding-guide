# 🎯 The Art of Prompt Engineering for Code

How to communicate with Claude for optimal code generation.

---

## Core Principles

### 1. Be Specific About Outcomes, Flexible About Implementation

**❌ Over-specified:**
```
Write a function called validateEmail that takes a string parameter 
named email, uses a regex pattern /^[a-zA-Z0-9.../, returns a boolean...
```

**✅ Outcome-focused:**
```
I need email validation that handles edge cases well. 
Return whether it's valid and why it failed if not.
```

Let Claude make implementation decisions. You focus on *what*, Claude figures out *how*.

### 2. Context is Everything

**❌ No context:**
```
Add user authentication
```

**✅ With context:**
```
Add user authentication to my Express API.
Using: TypeScript, Prisma, PostgreSQL
Existing patterns: [show example middleware]
Requirements: JWT tokens, refresh token rotation
```

### 3. Show, Don't Tell

**❌ Describing:**
```
Use the same error handling pattern we use elsewhere
```

**✅ Showing:**
```
Follow this error handling pattern:
```typescript
try {
  // operation
} catch (error) {
  logger.error('Operation failed', { error, context });
  throw new AppError('USER_FRIENDLY_MESSAGE', 500, error);
}
```

---

## Prompt Structures

### The Feature Request

```
Build [FEATURE] for [CONTEXT].

Requirements:
- [Must have 1]
- [Must have 2]

Nice to have:
- [Optional 1]

Constraints:
- [Limitation 1]

Start with [specific starting point].
```

### The Debugging Request

```
[SYMPTOM] when [TRIGGER].

Expected: [what should happen]
Actual: [what happens]

Error:
```
[full error message]
```

Code:
```[language]
[relevant code]
```

What I've tried: [attempts]
```

### The Refactoring Request

```
Refactor this [CODE TYPE] to be more [QUALITY].

Current:
```[language]
[code]
```

Goals:
- [Goal 1]
- [Goal 2]

Constraints:
- Keep [what must stay the same]
- Don't change [protected areas]
```

### The Review Request

```
Review this code for [FOCUS AREAS].

```[language]
[code]
```

Context: [what it does, where it runs]
Concerns: [specific worries]
```

---

## Power Techniques

### Chain of Thought

Ask Claude to think through the problem:
```
Before writing code, outline your approach:
1. What's the core challenge?
2. What patterns fit here?
3. What edge cases matter?

Then implement.
```

### Iterative Narrowing

Start broad, narrow down:
```
Round 1: "Build a user system"
Round 2: "Add email verification"
Round 3: "Handle the case where email already exists"
Round 4: "Add rate limiting to prevent abuse"
```

### Reference Anchoring

Anchor to known patterns:
```
Like how Stripe handles webhooks...
Similar to React Query's caching...
Following the repository pattern...
```

### Constraint Injection

Add constraints to guide output:
```
Requirements:
- Must work with Node 18
- No external dependencies
- Under 100 lines
- Handle 1000 requests/second
```

---

## Language Matters

### Action Verbs

| Verb | Claude Interprets As |
|------|---------------------|
| Build | Full implementation |
| Add | Extend existing code |
| Fix | Debug and correct |
| Refactor | Restructure without changing behavior |
| Optimize | Improve performance |
| Simplify | Reduce complexity |
| Review | Analyze and critique |
| Explain | Teach the concept |

### Precision Words

| Word | Meaning |
|------|---------|
| Exactly | No deviation allowed |
| Roughly | Approximate is fine |
| Always | Every single case |
| Usually | Most cases, exceptions okay |
| Must | Non-negotiable requirement |
| Should | Strong preference |
| Could | Nice to have |
| Consider | Think about, decide yourself |

### Hedging (Avoid)

**❌ Vague:**
```
Maybe add some error handling?
You could probably use async?
```

**✅ Clear:**
```
Add error handling for network failures.
Use async/await for the database calls.
```

---

## Meta-Prompts

### Request Format Preferences

```
When showing code:
- Include file paths
- Use TypeScript
- Add brief comments for complex logic
- Show imports

When explaining:
- Keep it concise
- Use bullet points
- Focus on the "why"
```

### Request Behavior Adjustments

```
After each code block, briefly explain:
- What it does
- Key decisions made
- Potential gotchas
```

```
Don't explain obvious things. Assume I'm a senior developer.
```

```
If you're unsure about a requirement, ask before implementing.
```

---

## Common Mistakes

### 1. Prompt Dumping

**❌ Everything at once:**
```
Build a todo app with user auth, teams, sharing, notifications,
offline support, real-time sync, mobile responsive, dark mode,
keyboard shortcuts, drag and drop, recurring tasks, subtasks...
```

**✅ Incremental:**
```
Build a basic todo app. Just tasks with CRUD.
[Later] Add user authentication.
[Later] Add team sharing.
```

### 2. Assumed Context

**❌ Assuming:**
```
Fix the bug in the user service.
```

**✅ Explicit:**
```
In my user service (Express + Prisma), the updateUser function 
throws when email is null. Here's the code: [code]
```

### 3. Style Over Substance

**❌ Prescriptive style:**
```
Use 2-space indentation, put braces on same line, use const for 
everything, alphabetize imports...
```

**✅ Meaningful constraints:**
```
Follow Airbnb style guide. Focus on:
- Clear naming
- Error handling
- Type safety
```

---

## The Golden Prompt

A template that works for most situations:

```
# Context
I'm building [PROJECT] using [STACK].
Current state: [what exists]

# Task
[What you need, outcome-focused]

# Requirements
- [Must have]

# Constraints  
- [Limitations]

# Starting Point
[Where to begin / what to touch]

# Output Format
[How you want the response]
```

---

## When Claude Misunderstands

### Redirect

```
That's not quite what I meant. I need:
[Clearer explanation]

Keep [what was good], but change [what was wrong].
```

### Constrain

```
You're overcomplicating this. Simpler approach:
- No [thing you don't want]
- Just [minimal thing you need]
```

### Reset

```
Let's step back. The core problem is:
[Reframe the problem]

What's the simplest solution?
```

---

Remember: Prompt engineering is a skill. You'll get better with practice. The best prompts are clear, contextual, and outcome-focused.
