# 🎨 Frontend SPA Workflow

Building modern frontend applications with vibe coding.

---

## Phase 1: Project Setup

### Initialize Project

```
Create a new [React/Vue/Svelte] project with:

- TypeScript
- [Tailwind / CSS Modules / Styled Components]
- Routing
- State management: [choice]
- Testing setup

Tooling:
- ESLint + Prettier
- Path aliases
- Environment variables
```

### Component Library Setup

```
Set up [component library] for my project:

Library: [shadcn/ui / Radix / MUI / etc.]
Theme:
- Primary color: [color]
- Style: [modern / minimal / corporate]

Configure theming and show me how to use components.
```

---

## Phase 2: Core Structure

### Routing Setup

```
Set up routing for my app:

Pages needed:
- / (home)
- /[page1]
- /[page2]
- /[resource]/:id

Include:
- Route guards for auth
- 404 handling
- Loading states
- Layout components
```

### State Management

```
Set up state management for my app:

State needs:
- User auth state
- [Domain] data
- UI state (modals, toasts)

Using: [Zustand / Redux / Jotai / etc.]

Create the store structure and show usage examples.
```

### API Layer

```
Create an API layer for my frontend:

Backend: [REST / GraphQL]
Base URL: [url]

Needs:
- API client setup
- Request/response types
- Error handling
- Authentication headers
- [React Query / SWR / Apollo] integration
```

---

## Phase 3: Building Features

### Page Component

```
Build the [PAGE NAME] page:

Purpose: [what the page does]

Components needed:
- [Component 1]
- [Component 2]

Data:
- Fetches: [what data]
- Displays: [how]

User interactions:
- [Interaction 1]
- [Interaction 2]
```

### Form Component

```
Build a form for [PURPOSE]:

Fields:
- [field1]: [type] (required/optional)
- [field2]: [type]

Validation rules:
- [rule 1]
- [rule 2]

On submit: [action]

Use [form library] with [validation library].
Include error states and loading state.
```

### Data Display

```
Build a [table/list/grid] to display [DATA]:

Data shape:
```typescript
[type definition]
```

Features:
- Pagination
- Sorting by [columns]
- Filtering by [fields]
- [Row actions / selection]

Fetch data with [React Query / SWR].
```

### Interactive Component

```
Build a [COMPONENT TYPE] component:

Purpose: [what it does]
User interactions:
- [Interaction 1]
- [Interaction 2]

States:
- Default
- Hover/Active
- Loading
- Error
- Success

Make it accessible (keyboard nav, ARIA).
```

---

## Phase 4: Polish

### Responsive Design

```
Make this component responsive:

```jsx
[component code]
```

Breakpoints:
- Mobile: [behavior]
- Tablet: [behavior]
- Desktop: [behavior]

Use [Tailwind breakpoints / CSS media queries].
```

### Loading States

```
Add loading states to this component:

```jsx
[component]
```

Show:
- Skeleton while loading
- Error state with retry
- Empty state when no data
- Partial loading for pagination
```

### Animations

```
Add animations to this component:

```jsx
[component]
```

Animate:
- [Element 1]: [animation type]
- [Element 2]: [animation type]

Use [Framer Motion / CSS transitions / etc.]
Keep it subtle and performant.
```

### Accessibility

```
Audit and fix accessibility for this component:

```jsx
[component]
```

Check:
- Keyboard navigation
- Screen reader support
- Color contrast
- Focus management
- ARIA attributes

Fix any issues found.
```

---

## Phase 5: Testing

### Component Tests

```
Write tests for this component:

```jsx
[component]
```

Test:
- Renders correctly
- User interactions work
- Loading/error states
- Props variations

Use [React Testing Library / Vue Test Utils].
```

### E2E Tests

```
Write E2E tests for this user flow:

Flow:
1. User [action 1]
2. User [action 2]
3. System [response]
4. User sees [result]

Use [Playwright / Cypress].
```

---

## Phase 6: Optimization

### Performance Audit

```
Audit performance of this component/page:

```jsx
[code]
```

Check:
- Unnecessary re-renders
- Bundle size impact
- Lazy loading opportunities
- Memoization needs

Suggest and implement optimizations.
```

### Code Splitting

```
Add code splitting to my app:

Current structure:
```
[route/page structure]
```

Implement:
- Route-based splitting
- Component lazy loading
- Suspense boundaries
- Loading fallbacks
```

### Bundle Analysis

```
My bundle is too large. Here's my setup:

```json
[package.json dependencies]
```

Help me:
- Identify heavy dependencies
- Find lighter alternatives
- Implement tree shaking
- Split vendors
```

---

## Common Patterns

### Modal/Dialog Pattern

```
Create a reusable modal system:

Requirements:
- Can be triggered from anywhere
- Supports different content
- Proper focus trap
- Escape to close
- Click outside to close

Use [your modal preference].
```

### Toast/Notification Pattern

```
Create a toast notification system:

Types: success, error, warning, info
Features:
- Auto-dismiss with timer
- Manual dismiss
- Queue multiple toasts
- Position: [top-right/bottom-center/etc.]
```

### Authentication Flow

```
Implement auth flow in my frontend:

Provider: [your auth]
Flows needed:
- Login
- Register
- Forgot password
- Logout
- Session refresh

Include:
- Protected routes
- Redirect after login
- Token storage
- Auth context
```
