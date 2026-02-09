# 🧪 Test-Driven Vibes Pattern

TDD with AI: let Claude write the tests first, then implement.

---

## The Pattern

Traditional TDD:
```
1. Write failing test
2. Implement code to pass
3. Refactor
```

Test-Driven Vibes:
```
1. Describe what you want → Claude writes tests
2. Review tests → Clarify requirements
3. Claude implements → Tests pass
4. Iterate on both
```

---

## Why It Works

Tests are a **specification**. When Claude writes tests first:
- Requirements become concrete
- Edge cases surface early
- Implementation has a target
- You review the spec, not just the code

---

## In Practice

### Step 1: Describe the Behavior

```
I need a password strength validator.

Requirements:
- Minimum 8 characters
- At least one uppercase
- At least one lowercase  
- At least one number
- At least one special character
- Returns strength score: weak/medium/strong
- Returns specific feedback for what's missing

Write the tests first.
```

### Step 2: Review the Tests

**Claude writes:**
```javascript
describe('validatePassword', () => {
  it('rejects passwords shorter than 8 characters', () => {
    expect(validatePassword('Short1!')).toEqual({
      valid: false,
      strength: 'weak',
      feedback: ['Password must be at least 8 characters']
    });
  });

  it('requires uppercase letter', () => {
    expect(validatePassword('lowercase1!')).toEqual({
      valid: false,
      strength: 'weak', 
      feedback: ['Password must contain an uppercase letter']
    });
  });

  // ... more tests
});
```

You review and refine:
```
Good tests. Also add:
- Test for common passwords ("password123")
- Test that multiple issues are reported together
- Test the strength scoring logic specifically
```

### Step 3: Implement to Pass

```
Now implement validatePassword to pass these tests.
```

### Step 4: Verify and Iterate

```
Run the tests. If any fail, fix the implementation.
Then add tests for [additional scenario].
```

---

## TDD Prompts

### Starting Fresh

```
I want to build [THING].

Behaviors:
- [behavior 1]
- [behavior 2]
- [edge case]

Write comprehensive tests first. Don't implement yet.
```

### Test-First Feature

```
New feature: [description]

Write tests that describe:
- Happy path
- Error conditions  
- Edge cases

I'll review the tests before implementation.
```

### Test-First Refactor

```
I'm refactoring [COMPONENT].

Current behavior (must not change):
- [behavior 1]
- [behavior 2]

Write tests that lock in current behavior.
Then we'll refactor safely.
```

---

## Test Quality Review

### Reviewing Claude's Tests

```
Review these tests:

```javascript
[tests Claude wrote]
```

Are they:
- Testing behavior, not implementation?
- Covering edge cases?
- Clear about what's being tested?
- Following AAA pattern?

Suggest improvements.
```

### Missing Test Cases

```
What test cases are we missing for:

```javascript
[current tests]
```

Implementation:
```javascript
[code being tested]
```

Add tests for gaps.
```

---

## Test Types

### Unit Test First

```
Write unit tests for a function that [does thing].

Input: [types/examples]
Output: [types/examples]

Test:
- Normal cases
- Boundary values
- Invalid inputs
- Error handling
```

### Integration Test First

```
Write integration tests for [API endpoint]:

Endpoint: [METHOD /path]
Request: [shape]
Response: [shape]

Test scenarios:
- Successful request
- Validation errors
- Auth required
- Not found
- Server error
```

### E2E Test First

```
Write E2E tests for [user flow]:

Flow:
1. User [action]
2. User [action]
3. System [response]

Test:
- Happy path works
- Errors are handled
- State persists correctly
```

---

## The Red-Green-Refactor Cycle

### Red: Write Failing Test

```
Write one test for the next requirement:

Requirement: [specific requirement]

Just one test, and confirm it would fail.
```

### Green: Minimal Implementation

```
Implement the minimum code to make this test pass:

```javascript
[failing test]
```

Don't over-engineer. Just pass the test.
```

### Refactor: Improve

```
The tests pass. Now refactor for:
- Readability
- Performance (if needed)
- Duplication removal

Keep all tests green.
```

---

## TDD for Debugging

### Write Test for Bug

```
Bug: [description of bug]
Steps to reproduce: [steps]

Write a test that reproduces this bug (currently failing).
```

### Fix with Confidence

```
Here's the failing test:

```javascript
[test that reproduces bug]
```

Fix the implementation. The test should pass after.
```

---

## Anti-Patterns

### ❌ Test Implementation Details

```javascript
// Bad: tests HOW not WHAT
it('calls database.save once', () => {
  createUser(data);
  expect(database.save).toHaveBeenCalledTimes(1);
});
```

### ✅ Test Behavior

```javascript
// Good: tests the outcome
it('creates a user that can be retrieved', async () => {
  const user = await createUser(data);
  const retrieved = await getUser(user.id);
  expect(retrieved.email).toBe(data.email);
});
```

---

## Pro Tips

### 1. Let Tests Define the Interface

Tests reveal what your API should look like:
```javascript
// This test suggests the function signature
expect(formatCurrency(1000, 'USD')).toBe('$1,000.00');
```

### 2. Tests as Documentation

Good tests explain how to use the code:
```
Make these tests readable enough to serve as documentation.
```

### 3. Start with the Happy Path

Don't get lost in edge cases immediately:
```
First, just test the main success scenario.
We'll add edge cases after.
```

### 4. One Assertion Per Test

```
Split this test into multiple tests, one assertion each.
```

### 5. Run Frequently

```
After each change, run tests and tell me results.
```
