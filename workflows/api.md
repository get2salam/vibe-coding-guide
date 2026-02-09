# 🔌 API Development Workflow

Building robust APIs with vibe coding.

---

## Phase 1: API Design

### Design First

```
Design a REST API for [DOMAIN].

Resources:
- [Resource 1]: [description]
- [Resource 2]: [description]

Operations needed:
- [Operation 1]
- [Operation 2]

Show me:
- Endpoint list
- Request/response shapes
- Status codes
- Authentication approach
```

### OpenAPI Spec

```
Create an OpenAPI 3.0 spec for this API:

Endpoints:
- [List endpoints]

Include:
- Request/response schemas
- Authentication
- Error responses
- Examples
```

### Data Modeling

```
Design the data model for this API:

Entities:
- [Entity 1]: [fields]
- [Entity 2]: [fields]

Relationships:
- [How they connect]

Create:
- Database schema
- API response DTOs
- Mapping between them
```

---

## Phase 2: Implementation

### Setup API Framework

```
Set up a [FRAMEWORK] API with:

Features:
- TypeScript
- Request validation
- Error handling middleware
- Logging
- CORS configuration

Show me the project structure and boilerplate.
```

### CRUD Endpoint

```
Implement CRUD for [RESOURCE]:

Schema:
```[language]
[schema/model]
```

Implement:
- GET /[resources] (list with pagination)
- GET /[resources]/:id
- POST /[resources]
- PUT /[resources]/:id
- DELETE /[resources]/:id

Include validation and error handling.
```

### Authentication

```
Add authentication to my API:

Method: [JWT / OAuth / API Key]
Requirements:
- [Requirement 1]
- [Requirement 2]

Implement:
- Auth middleware
- Login/register endpoints
- Token refresh
- Role-based access control
```

### Complex Endpoint

```
Build an endpoint for [COMPLEX OPERATION]:

Business logic:
- [Step 1]
- [Step 2]
- [Step 3]

Needs to:
- [Requirement]
- [Handle edge case]

Show the controller, service, and any helpers needed.
```

---

## Phase 3: Robustness

### Validation

```
Add comprehensive validation to this endpoint:

```[language]
[endpoint code]
```

Validate:
- Input types
- Required fields
- Format (email, URL, etc.)
- Business rules

Return clear error messages.
```

### Error Handling

```
Implement consistent error handling for my API.

Current endpoints:
```[language]
[sample endpoint]
```

Create:
- Error classes hierarchy
- Error handling middleware
- Consistent error response format
- Proper HTTP status codes
```

### Rate Limiting

```
Add rate limiting to my API:

Requirements:
- Global limit: [X] requests per [time]
- Per-endpoint limits for [heavy endpoints]
- Authenticated users get higher limits

Stack: [what you're using]
```

### Caching

```
Add caching to these endpoints:

```[language]
[endpoints]
```

Strategy:
- [Endpoint 1]: [cache strategy and TTL]
- [Endpoint 2]: [cache strategy and TTL]

Consider cache invalidation for write operations.
```

---

## Phase 4: Testing

### Unit Tests

```
Write unit tests for this service:

```[language]
[service code]
```

Mock:
- Database calls
- External services

Test:
- Happy paths
- Error conditions
- Edge cases
```

### Integration Tests

```
Write integration tests for these endpoints:

```[language]
[endpoints]
```

Test:
- Full request/response cycle
- Database interactions
- Authentication
- Validation errors

Use [test framework] with [test database strategy].
```

### API Contract Tests

```
Create contract tests for my API:

OpenAPI spec:
```yaml
[spec]
```

Verify:
- Responses match schema
- Required fields present
- Status codes correct
```

---

## Phase 5: Documentation

### API Documentation

```
Generate API documentation:

Endpoints:
```[language]
[endpoint code]
```

Create:
- Endpoint descriptions
- Request/response examples
- Authentication guide
- Error code reference

Format for [Swagger UI / Readme / Postman].
```

### Postman Collection

```
Create a Postman collection for my API:

Endpoints:
- [List endpoints]

Include:
- Environment variables
- Pre-request scripts for auth
- Example requests
- Tests for responses
```

---

## Phase 6: Production

### Monitoring

```
Add monitoring to my API:

Track:
- Request latency
- Error rates
- Endpoint usage
- Resource utilization

Stack: [your monitoring tools]
```

### Health Checks

```
Implement health check endpoints:

Dependencies:
- Database
- Redis
- External services

Create:
- /health (basic)
- /health/ready (full dependency check)
- /health/live (kubernetes liveness)
```

### Performance Optimization

```
Optimize this slow endpoint:

```[language]
[endpoint code]
```

Current: [X]ms average
Target: [Y]ms

Analyze and optimize:
- Database queries
- Memory usage
- External calls
- Response size
```

---

## Common Patterns

### Pagination

```
Add pagination to this list endpoint:

```[language]
[endpoint]
```

Implement:
- Offset-based and/or cursor-based
- Page size limits
- Total count (optional)
- Next/previous links
```

### Filtering & Sorting

```
Add filtering and sorting to this list endpoint:

```[language]
[endpoint]
```

Filters needed:
- [Field 1]: [exact / range / search]
- [Field 2]: [type]

Sort options: [fields]

Make it type-safe and SQL-injection proof.
```

### Versioning

```
Add API versioning to my project:

Current structure:
```
[file structure]
```

Strategy: [URL path / header / query param]

Show me how to:
- Version endpoints
- Deprecate old versions
- Maintain multiple versions
```
