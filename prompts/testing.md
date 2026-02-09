# 🧪 Testing Prompts

Generate comprehensive tests with minimal effort.

---

## The Test Generation Template

```
Write tests for this code:

```[language]
[code to test]
```

Test framework: [jest/pytest/mocha/etc.]
Cover:
- Happy path
- Edge cases
- Error conditions
```

---

## Test Types

### 🔬 Unit Tests

```
Write unit tests for this function:

```[language]
[function code]
```

Test:
- Normal inputs
- Boundary values
- Invalid inputs
- Return value types

Mock any dependencies.
```

### 🔗 Integration Tests

```
Write integration tests for this API endpoint:

```[language]
[route handler code]
```

Endpoint: [METHOD /path]
Test:
- Successful requests
- Validation errors
- Authentication/authorization
- Database interactions

Use [test framework] with [http testing library].
```

### 🎭 E2E Tests

```
Write E2E tests for this user flow:

1. User goes to [page]
2. User [action]
3. System [response]
4. User sees [result]

Use [Playwright/Cypress/etc.].
Include:
- Happy path
- Main error scenarios
- Accessibility checks
```

### 📸 Snapshot Tests

```
Create snapshot tests for this React component:

```jsx
[component code]
```

Test these variants:
- Default props
- [Variant 1]
- [Variant 2]
- Loading state
- Error state
```

---

## Specific Testing Scenarios

### Test Async Code

```
Write tests for this async function:

```[language]
[async code]
```

Cover:
- Successful async operations
- Timeout scenarios
- Retry logic
- Concurrent calls
- Error handling

Show proper async test patterns.
```

### Test with Mocks

```
This code has external dependencies:

```[language]
[code with dependencies]
```

Dependencies:
- [Dependency 1]: [what it does]
- [Dependency 2]: [what it does]

Write tests with proper mocks for each dependency.
```

### Test Error Handling

```
Write tests that verify error handling:

```[language]
[code with try/catch or error handling]
```

Test that:
- Correct errors are thrown for [condition]
- Errors contain useful information
- System recovers gracefully
- Error callbacks are invoked
```

### Test Database Operations

```
Write tests for these database operations:

```[language]
[repository/data access code]
```

Include:
- CRUD operation tests
- Transaction tests
- Constraint violation handling
- Connection error handling

Use [in-memory DB / test containers / mocks].
```

---

## Test-Driven Development

### TDD: Start with Tests

```
I want to build [feature description].

Write the tests first (TDD style).
Define:
- What the interface should look like
- Expected behaviors
- Edge cases

Then I'll implement to make tests pass.
```

### Red-Green-Refactor

```
Let's do TDD for [feature]:

1. Write a failing test for the first requirement
2. I'll implement minimum code to pass
3. Help me refactor
4. Next test

Start with the simplest test case.
```

---

## Test Quality

### Review Test Coverage

```
Review these tests for completeness:

Tests:
```[language]
[existing tests]
```

Code being tested:
```[language]
[implementation]
```

What's missing? What edge cases aren't covered?
```

### Improve Flaky Tests

```
This test is flaky:

```[language]
[flaky test]
```

It fails intermittently with: [error/behavior]

Identify the cause and fix it.
```

### Test Naming and Organization

```
Improve the naming and organization of these tests:

```[language]
[tests with poor structure]
```

Apply:
- Descriptive test names
- Arrange-Act-Assert pattern
- Logical grouping
- Setup/teardown where appropriate
```

---

## Testing Utilities

### Generate Test Data

```
Generate test fixtures/factories for this model:

```[language]
[model/schema]
```

Create:
- Valid data factory
- Edge case variations
- Invalid data for error testing

Use [faker.js / factory pattern / etc.].
```

### Create Test Helpers

```
I'm repeating this test setup a lot:

```[language]
[repeated setup code]
```

Create reusable test helpers that:
- Set up [common scenario]
- Clean up after tests
- Provide [utilities]
```

---

## Coverage Analysis

### Identify Coverage Gaps

```
Here's my code and tests:

Code:
```[language]
[implementation]
```

Tests:
```[language]
[current tests]
```

What code paths aren't covered? Write tests for the gaps.
```

### Meaningful Coverage

```
My coverage is [X]% but I'm not confident in the tests.

Tests:
```[language]
[tests]
```

Are these testing behavior or just lines? How can I make them more meaningful?
```

---

## Pro Tips

1. **Test behavior, not implementation** - Tests shouldn't break when you refactor
2. **One assertion per test** - Makes failures easy to diagnose
3. **Test names are documentation** - "should_return_error_when_email_invalid"
4. **Arrange-Act-Assert** - Structure makes tests readable
5. **Don't test the framework** - Focus on your code
6. **Flaky tests are worse than no tests** - Fix or delete them
