# 🚀 Feature Development Prompts

Battle-tested prompts for building new features with Claude.

---

## The Golden Template

```
I need to build [FEATURE] for my [PROJECT TYPE].

Context:
- Stack: [your stack]
- Current state: [what exists]
- Goal: [what success looks like]

Requirements:
- [requirement 1]
- [requirement 2]

Start with [specific starting point] and we'll iterate.
```

---

## Feature Types

### 🔐 Authentication Feature

```
Build user authentication for my Express API.

Context:
- Using Express 4.x with TypeScript
- PostgreSQL database with Prisma ORM
- Need JWT-based auth

Requirements:
- Sign up with email/password
- Login returns access + refresh tokens
- Password hashing with bcrypt
- Token refresh endpoint
- Logout (blacklist token)

Start with the Prisma schema and auth routes.
```

### 📊 CRUD Feature

```
I need a complete CRUD for [RESOURCE] in my [FRAMEWORK] app.

The resource has these fields:
- [field1]: [type] (required)
- [field2]: [type] (optional)
- [field3]: [type] (default: [value])

Include:
- Input validation
- Error handling
- Pagination for list endpoint
- Soft delete

Follow the existing patterns in my codebase.
```

### 🔍 Search Feature

```
Add search functionality to my [RESOURCE] list.

Current setup:
- [describe current list implementation]

Search requirements:
- Full-text search on [fields]
- Filters for [filter options]
- Sort by [sort options]
- Debounced frontend input

Show me the backend query first, then frontend component.
```

### 📧 Notification Feature

```
Build a notification system for my app.

Types of notifications needed:
- In-app notifications
- Email notifications (using [provider])
- [Optional: push/SMS]

Triggers:
- [Event 1] → [notification type]
- [Event 2] → [notification type]

Include a notification preferences table for users.
```

---

## Iteration Prompts

### Refining the Implementation

```
This is good, but:
- [Change 1]
- [Change 2]

Keep everything else the same.
```

### Adding Edge Cases

```
Now handle these edge cases:
- What if [edge case 1]?
- What if [edge case 2]?

Add appropriate error messages.
```

### Performance Optimization

```
This works but might be slow at scale. Optimize for:
- [Expected data volume]
- [Expected concurrent users]

Consider caching and query optimization.
```

### Adding Tests

```
Add tests for this feature:
- Unit tests for the core logic
- Integration tests for the API endpoints
- Cover the main happy path and key edge cases
```

---

## Anti-Patterns to Avoid

### ❌ Too Vague

```
Make a user system
```

### ✅ Better

```
Build user registration and login for my Next.js app using NextAuth with email/password and Google OAuth.
```

### ❌ Over-Specified

```
Create a function called createUser that takes an object with email as string and password as string, validates email with regex /^[a-zA-Z0-9.../, hashes password using bcrypt with 12 rounds, creates a database record...
```

### ✅ Better

```
Create user registration with email/password. Use bcrypt for hashing, validate email format, return the user without the password hash.
```

---

## Pro Tips

1. **Start small**: Get the core working, then add complexity
2. **Show, don't tell**: If you have existing patterns, share them
3. **Name your constraints**: "Must work with existing User model"
4. **Define success**: "Done when I can [action] and see [result]"
