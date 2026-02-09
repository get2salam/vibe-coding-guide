# 🏗️ Full-Stack App Workflow

Building complete applications with vibe coding.

---

## Phase 1: Planning

### Define the Product

```
I'm building [APP NAME] - a [description].

Target users: [who]
Core problem: [what pain point]
Solution: [how it helps]

MVP features:
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]

Success metrics:
- [Metric 1]
- [Metric 2]
```

### Choose the Stack

```
Recommend a full-stack for my project:

App type: [SaaS / marketplace / social / tool]
Scale: [MVP / growth / enterprise]
Team: [size and skills]

Priorities:
- Development speed
- Scalability
- Maintainability
- Cost

Compare options and recommend with reasoning.
```

### Architecture Design

```
Design the architecture for [APP]:

Features:
- [Feature list]

Requirements:
- Users: [expected scale]
- Real-time: [yes/no]
- File uploads: [yes/no]
- Payment: [yes/no]

Create:
- System diagram
- Service boundaries
- Data flow
- Tech stack per component
```

---

## Phase 2: Backend Foundation

### Database Schema

```
Design the database for [APP]:

Core entities:
- User: [fields]
- [Entity 2]: [fields]
- [Entity 3]: [fields]

Relationships and constraints.

Using: [PostgreSQL / MongoDB / etc.]
ORM: [Prisma / Drizzle / etc.]
```

### API Structure

```
Set up the API for [APP]:

Framework: [Express / Fastify / Hono / etc.]
Structure:
- Routes organization
- Middleware stack
- Error handling
- Validation

Create the foundation with one example endpoint.
```

### Authentication System

```
Implement auth for [APP]:

Method: [email+password / OAuth / magic link]
Features:
- Sign up / Login / Logout
- Password reset
- Email verification
- Session management

Include rate limiting and security best practices.
```

---

## Phase 3: Frontend Foundation

### Project Setup

```
Set up frontend for [APP]:

Framework: [Next.js / Remix / SvelteKit / etc.]
Styling: [Tailwind + shadcn/ui / etc.]
State: [React Query + Zustand / etc.]

Create:
- Project structure
- Theme configuration
- Layout components
- API client setup
```

### Core Pages

```
Build the core pages for [APP]:

Pages:
- Landing page
- Auth pages (login/signup)
- Dashboard
- [Main feature page]

Start with layouts and navigation, then page content.
```

### Shared Components

```
Create shared components for [APP]:

Need:
- Button (variants)
- Input (with validation)
- Card
- Modal
- Toast notifications
- Loading states

Use [component library] as base, customize to brand.
```

---

## Phase 4: Feature Development

### Feature Sprint Template

```
Build [FEATURE NAME]:

User stories:
- As a [user], I want to [action] so that [benefit]

Acceptance criteria:
- [ ] [Criterion 1]
- [ ] [Criterion 2]

Build in order:
1. Database changes
2. API endpoints
3. Frontend components
4. Integration
5. Tests
```

### Real-Time Feature

```
Add real-time [FEATURE] to [APP]:

Use case: [what needs to be real-time]
Users involved: [how many concurrent]

Implement with [WebSockets / SSE / Socket.io / Pusher]

Show:
- Backend event emission
- Frontend subscription
- Reconnection handling
- Optimistic updates
```

### File Upload Feature

```
Add file uploads to [APP]:

Files: [images / documents / any]
Max size: [limit]
Storage: [S3 / R2 / local]

Implement:
- Upload endpoint with validation
- Progress indication
- Image optimization (if applicable)
- Access control
```

### Payment Integration

```
Add payments to [APP]:

Provider: [Stripe / etc.]
Model: [subscription / one-time / usage-based]

Implement:
- Checkout flow
- Webhook handling
- Subscription management
- Invoice access

Include test mode setup.
```

---

## Phase 5: Quality

### Testing Strategy

```
Create testing strategy for [APP]:

Backend:
- Unit tests for services
- Integration tests for API
- Database tests

Frontend:
- Component tests
- Integration tests
- E2E for critical flows

Show me the setup and examples of each.
```

### Error Handling

```
Implement comprehensive error handling:

Backend:
- Error classes
- Centralized handler
- Logging
- Client-safe messages

Frontend:
- Global error boundary
- API error handling
- Form validation errors
- Toast notifications
```

### Performance

```
Performance checklist for [APP]:

Backend:
- [ ] Database query optimization
- [ ] Caching strategy
- [ ] Connection pooling

Frontend:
- [ ] Code splitting
- [ ] Image optimization
- [ ] Caching headers
- [ ] Core Web Vitals

Implement optimizations for each.
```

---

## Phase 6: Deployment

### Infrastructure Setup

```
Set up infrastructure for [APP]:

Provider: [Vercel / AWS / Railway / etc.]

Components:
- Frontend hosting
- API hosting
- Database
- File storage
- Email service

Environment: [staging + production]

Create deployment configuration.
```

### CI/CD Pipeline

```
Set up CI/CD for [APP]:

Repo: [GitHub / GitLab]

Pipeline:
- Lint and type check
- Run tests
- Build
- Deploy to staging
- Deploy to production (manual trigger)

Create the workflow file.
```

### Monitoring & Observability

```
Set up monitoring for [APP]:

Track:
- Application errors
- Performance metrics
- User analytics
- Server health

Tools: [Sentry / Datadog / etc.]

Set up and show me key dashboards/alerts.
```

---

## Phase 7: Launch

### Pre-Launch Checklist

```
Pre-launch checklist for [APP]:

Security:
- [ ] Auth tested
- [ ] Input validation
- [ ] Rate limiting
- [ ] HTTPS
- [ ] Secrets management

Legal:
- [ ] Privacy policy
- [ ] Terms of service
- [ ] Cookie consent

Performance:
- [ ] Load tested
- [ ] CDN configured
- [ ] Database indexed

Review and flag any issues.
```

### Launch Plan

```
Create launch plan for [APP]:

Timeline: [date]
Channels: [where to announce]
Initial users: [beta / public]

Prepare:
- Announcement copy
- Demo content
- Support documentation
- Rollback plan
```

---

## Post-Launch

### Iteration Cycle

```
We've launched! Current state:
- Users: [count]
- Feedback: [summary]

Next iteration priorities:
1. [Bug / issue to fix]
2. [Feature request]
3. [Performance improvement]

Let's tackle [first item].
```
