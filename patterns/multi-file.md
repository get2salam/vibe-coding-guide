# 📁 Multi-File Orchestration Pattern

How to build features that span multiple files with vibe coding.

---

## The Challenge

Real features touch multiple files:
- Database schema
- API route
- Service logic
- Frontend component
- Types/interfaces
- Tests

How do you vibe code across all of them coherently?

---

## The Pattern

### 1. Plan the Structure First

```
I'm building [FEATURE].

It will need:
- Database: [what tables/changes]
- API: [what endpoints]
- Frontend: [what components]
- Shared: [types, utils]

Outline the files we'll create/modify before we start coding.
```

**Claude responds with:**
```
Here's the plan:

1. Database (prisma/schema.prisma)
   - Add Comment model
   - Add relation to Post

2. API
   - src/routes/comments.ts (CRUD routes)
   - src/services/commentService.ts (business logic)

3. Frontend
   - components/CommentList.tsx
   - components/CommentForm.tsx
   - hooks/useComments.ts

4. Types
   - types/comment.ts (shared types)

Let's start with [suggested order].
```

### 2. Build in Dependency Order

```
Start with the database schema and types.
```

[Claude creates schema + types]

```
Now the service layer.
```

[Claude creates services, importing the types]

```
Now the API routes.
```

[Claude creates routes, using the services]

```
Finally, the frontend components.
```

[Claude creates components, calling the API]

### 3. Show Me Everything

```
Show me all the files together with their paths.
```

**Claude shows:**
```
// prisma/schema.prisma
[schema code]

// src/types/comment.ts
[types]

// src/services/commentService.ts
[service code]

// src/routes/comments.ts
[route code]

// src/components/CommentList.tsx
[component code]

// src/components/CommentForm.tsx
[component code]
```

---

## Orchestration Strategies

### Strategy 1: Top-Down

Start from the user interface, work backward:

```
1. What does the user see? → Design component
2. What data does it need? → Define API
3. What logic is required? → Build service
4. What data is stored? → Create schema
```

Good for: UI-driven features, user stories

### Strategy 2: Bottom-Up

Start from the data, work forward:

```
1. What data do we store? → Design schema
2. What operations on data? → Build service
3. How to expose it? → Create API
4. How to display it? → Build components
```

Good for: Data-driven features, migrations

### Strategy 3: Contract-First

Start from the interface/API:

```
1. Define the API contract (OpenAPI, types)
2. Build backend to fulfill contract
3. Build frontend to consume contract
```

Good for: Team collaboration, API-first design

---

## Managing Changes Across Files

### Adding a Field

```
Add a `priority` field to Task:

Needs changes in:
- Database schema
- API DTOs
- Service validation
- Frontend form
- Frontend display

Show me all changes.
```

### Refactoring Across Files

```
Rename `userId` to `authorId` across the codebase.

Affected files:
- [list what you know]

Find all usages and update them consistently.
```

### Feature Flag Across Stack

```
Add feature flag for [FEATURE]:

- Backend: check flag before enabling endpoint
- Frontend: conditionally render UI
- Config: environment variable
```

---

## Keeping Files in Sync

### Types as Source of Truth

```
Here are our shared types:

```typescript
// types/task.ts
interface Task {
  id: string;
  title: string;
  status: 'todo' | 'doing' | 'done';
  createdAt: Date;
}
```

Generate:
- Prisma schema matching these types
- Zod validation schema
- API response type
```

### Consistency Checks

```
Review these files for consistency:

Schema:
[paste schema]

Types:
[paste types]

API response:
[paste response handler]

Are there any mismatches?
```

---

## Multi-File Templates

### Full-Stack Feature Template

```
Build [FEATURE] full-stack:

Requirements:
- [requirement 1]
- [requirement 2]

Create these files:
1. prisma/schema.prisma changes
2. src/types/[feature].ts
3. src/services/[feature]Service.ts
4. src/routes/[feature].ts
5. app/components/[Feature]/*.tsx
6. app/hooks/use[Feature].ts

Build each file with proper imports between them.
```

### CRUD Feature Template

```
Generate full CRUD for [RESOURCE]:

Model: [describe fields]

Generate:
- Database model
- Service with create, read, update, delete, list
- API routes with validation
- React hooks (useResource, useResources)
- List component with pagination
- Form component for create/edit
- Delete confirmation
```

---

## Pro Tips

### 1. Show the File Tree

```
After building, show me the file tree:

```
src/
  features/
    comments/
      __tests__/
      components/
      hooks/
      services/
      types/
      index.ts
```

### 2. Generate Index Files

```
Create index.ts files that re-export for clean imports.
```

### 3. Request Imports Check

```
Verify all imports are correct between files.
```

### 4. Build Incrementally

Don't try to generate 10 files at once. Go file by file, testing each.

### 5. Use Consistent Naming

```
Use these naming conventions:
- Services: [resource]Service.ts
- Routes: [resource].routes.ts
- Components: [Resource][Action].tsx
- Hooks: use[Resource].ts
```
