# 📚 Context Loading Pattern

How to give Claude the context it needs for accurate, project-aware code.

---

## The Problem

Claude doesn't know your codebase. Without context, you get:
- Generic solutions that don't fit
- Wrong naming conventions
- Missed patterns you already have
- Incompatible implementations

---

## The Solution

Load context strategically at the start of your session.

---

## Context Loading Methods

### 1. AGENTS.md (Best Practice)

Create an `AGENTS.md` in your repo root:

```markdown
# AGENTS.md

## Project Context
E-commerce platform for artisan products.
Monorepo: apps/web (Next.js), apps/api (Express), packages/shared

## Tech Stack
- Frontend: Next.js 14 (App Router), TypeScript, Tailwind, shadcn/ui
- Backend: Express, Prisma, PostgreSQL
- Auth: NextAuth with credentials + Google
- State: React Query + Zustand

## Conventions
- Components in PascalCase, hooks use `use` prefix
- API routes: /api/v1/[resource]
- All dates in UTC, displayed in user timezone
- Error responses: { error: string, code: string, details?: object }

## File Structure
```
apps/
  web/
    app/           # App router pages
    components/    # Shared components
    lib/           # Utilities
  api/
    src/
      routes/      # Express routes
      services/    # Business logic
      middleware/  # Express middleware
```

## Current Sprint
- Building seller dashboard
- Implementing order management
- Adding review system

## Important Patterns
- Use `useQuery` for fetching, `useMutation` for actions
- All forms use react-hook-form + zod
- Prices stored in cents, displayed with formatPrice()
```

### 2. Inline Context Block

At the start of a conversation:

```
Context for this session:

Project: Task management SaaS
Stack: Remix, Prisma, PostgreSQL, Tailwind
Current file structure:
- app/routes/ - Remix routes
- app/models/ - Prisma queries
- app/components/ - UI components

Conventions:
- Loader functions fetch data
- Action functions handle mutations
- Use $form for progressive enhancement

I'm working on: the team invitation system
```

### 3. Show, Don't Tell

Instead of describing patterns, show them:

```
Here's how we do API routes in this project:

```typescript
// src/routes/users.ts
export const userRoutes = createRouter()
  .get('/', listUsers)
  .get('/:id', getUser)
  .post('/', validateBody(createUserSchema), createUser)
  .put('/:id', validateBody(updateUserSchema), updateUser)
  .delete('/:id', deleteUser);
```

Follow this pattern for the new [resource] routes.
```

### 4. Reference Existing Code

```
Build a new component similar to this one:

```tsx
// components/UserCard.tsx
[paste existing component]
```

But for displaying [different thing].
```

---

## What Context to Load

### Always Include

- **Stack** - Frameworks, languages, versions
- **Conventions** - Naming, file organization, patterns
- **Current task** - What you're building right now

### Include When Relevant

- **Database schema** - For data-related tasks
- **API contracts** - For integration work
- **Example code** - For pattern matching
- **Error messages** - For debugging

### Skip

- Full file contents (unless necessary)
- Unrelated parts of the codebase
- Historical decisions (unless they constrain current work)

---

## Context Maintenance

### Session Start

```
[Load AGENTS.md or paste context block]
"I'm continuing work on [feature]. Last session we [summary]."
```

### Mid-Session Refresh

```
"Quick context refresh: we're building [X] for the [project].
Current state: [what exists]
Next step: [what we're doing now]"
```

### Context Correction

```
"Actually, I should clarify: [correction].
Adjust the code to match."
```

---

## Context Size Management

### Too Little Context

**Symptom:** Claude suggests things that don't fit your project

**Fix:** Add relevant constraints and examples

### Too Much Context

**Symptom:** Responses are slow, Claude gets confused

**Fix:** Summarize, keep only what's relevant to current task

### Sweet Spot

- 500-2000 words of context
- Specific to current task
- Includes examples of patterns to follow

---

## Templates

### Quick Context Template

```
Stack: [frameworks]
Working on: [feature]
Pattern to follow: [brief or show example]
```

### Full Context Template

```
# Project Context

## Overview
[What is this project]

## Tech Stack
[List technologies]

## Current Task
[What we're building]

## Relevant Patterns
[Code examples or descriptions]

## Constraints
[Any limitations or requirements]
```

---

## Pro Tips

1. **Update AGENTS.md regularly** - Keep it current with sprint goals
2. **Load context first, ask questions second** - Set the stage before coding
3. **Correct early** - If Claude misses context, fix it immediately
4. **Be specific about versions** - "React 18" vs "React" matters
5. **Show existing code** - Examples > descriptions
