# 🛡️ Security Review Pattern

Use AI to systematically audit code for vulnerabilities before shipping.

---

## The Pattern

```
1. Implement feature → Claude builds it
2. Switch hats → Ask Claude to attack its own code
3. Fix vulnerabilities → Iterate until hardened
4. Verify with tests → Write security-focused test cases
```

The key insight: Claude is excellent at finding vulnerabilities in code — including code it just wrote. By explicitly switching from "builder" to "attacker" mode, you get a thorough review.

---

## Why It Works

When Claude writes code, it focuses on making things work. When you ask it to *review* code, it focuses on what could go wrong. These are different cognitive modes:

- **Builder mode**: "How do I make this function correctly?"
- **Attacker mode**: "How would I exploit this to steal data?"

Separating these modes produces better results than trying to do both at once.

---

## In Practice

### Step 1: Build the Feature

```
Build a user profile API endpoint:
- GET /api/profile/:id — returns user profile
- PUT /api/profile/:id — updates user profile
- Include name, email, bio, avatar_url fields
- Use Express and PostgreSQL
```

Claude implements the feature normally.

### Step 2: Switch to Attacker Mode

```
Now put on your security hat. You're a penetration tester.

Review the code you just wrote and try to find:

1. Can I access other users' profiles without auth?
2. Can I modify fields I shouldn't (role, is_admin)?
3. Can I inject SQL through any input?
4. Can I XSS through the bio or name fields?
5. Can I enumerate all user IDs?
6. Does the error handling leak sensitive info?
7. Is there any path traversal risk in avatar_url?
8. Can I DoS this endpoint?

For each finding, show the attack and the fix.
```

### Step 3: Apply Fixes

Claude identifies vulnerabilities and provides fixes:

```
Apply all the security fixes you identified.
After each fix, explain what attack it prevents.
```

### Step 4: Write Security Tests

```
Write test cases that verify each vulnerability is fixed:

- Test that auth is required
- Test that users can't access other profiles
- Test that mass assignment is blocked
- Test SQL injection attempts are rejected
- Test XSS payloads are sanitized
- Test rate limiting works

Each test should attempt the attack and verify it fails.
```

---

## Security Review Prompts

### Quick Review (5 minutes)

```
Quick security scan of this code:

```[language]
[paste code]
```

Flag: injection risks, auth issues, data leaks, hardcoded secrets.
Rate severity: critical/high/medium/low.
```

### Deep Review (thorough)

```
Perform a thorough security audit of this module:

```[language]
[paste code]
```

Check against OWASP Top 10:
1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable Components
7. Auth Failures
8. Data Integrity Failures
9. Logging Failures
10. SSRF

For each finding:
- Vulnerability description
- Proof of concept (how to exploit)
- Severity rating
- Recommended fix with code
```

### Dependency Review

```
Review these dependencies for known security issues:

```json
[paste package.json / requirements.txt / Cargo.toml]
```

Check for:
- Known CVEs in current versions
- Abandoned/unmaintained packages
- Packages with excessive permissions
- Better-maintained alternatives
```

### Configuration Review

```
Review this configuration for security issues:

```yaml
[paste config]
```

Check for:
- Overly permissive settings
- Default credentials
- Debug mode enabled
- Exposed internal endpoints
- Missing TLS configuration
- Insecure CORS settings
```

---

## The Attack Catalog

Use these as prompts to test specific attack vectors:

### Authentication Attacks

```
Try to bypass authentication in this code:
- Can I forge a session/token?
- Can I reuse an expired token?
- Is there a timing attack on password comparison?
- Can I brute force without being rate limited?
- Can I use a null/empty token?
```

### Authorization Attacks

```
Try to escalate privileges:
- Can a regular user access admin endpoints?
- Can user A modify user B's data?
- Are there IDOR vulnerabilities (changing IDs in requests)?
- Can I bypass role checks by modifying my own user object?
- Are there any endpoints missing auth middleware?
```

### Input Attacks

```
Try to inject malicious input:
- SQL: ' OR '1'='1'; DROP TABLE users;--
- XSS: <script>alert(document.cookie)</script>
- Command: ; rm -rf / ;
- Path traversal: ../../etc/passwd
- Template injection: {{7*7}}
- Header injection: \r\nX-Injected: true
- JSON injection: nested objects where strings expected
```

### Data Exfiltration

```
Try to extract sensitive data:
- Are stack traces visible to users?
- Do error messages reveal schema/query info?
- Can I get user data from verbose API responses?
- Are sensitive fields (password_hash) in responses?
- Can I enumerate resources via predictable IDs?
- Is there information leakage in HTTP headers?
```

---

## Review Workflow for Teams

### Pre-Merge Security Check

```
I'm about to merge this PR. Quick security review:

Changes:
- New endpoint: POST /api/payments
- Modified: user authentication middleware
- Added: file upload for receipts

Focus on:
- Payment logic correctness
- Auth changes don't introduce regressions
- File upload security
```

### Post-Incident Review

```
We had a security incident:

What happened: [description]
Impact: [what was affected]
Root cause: [if known]

Review the relevant code and:
1. Identify the exact vulnerability
2. Provide an immediate fix
3. Suggest additional hardening
4. Recommend tests to prevent regression
```

---

## Integrating Security Reviews into Your Workflow

### AGENTS.md Security Section

```markdown
## Security Review Requirements

Before merging any feature:
1. Run automated security scan (npm audit / bandit)
2. Review for OWASP Top 10 vulnerabilities
3. Ensure all inputs are validated server-side
4. Verify auth/authz on new endpoints
5. Check for hardcoded secrets
6. Write at least one security-focused test
```

### Security Review Checklist Template

```markdown
## Security Review: [Feature Name]

### Auth & Access
- [ ] Authentication required on all non-public endpoints
- [ ] Authorization checks verify resource ownership
- [ ] No privilege escalation paths

### Input/Output
- [ ] All inputs validated (type, length, format)
- [ ] Outputs properly encoded/escaped
- [ ] Error messages don't leak internals

### Data
- [ ] Sensitive data encrypted
- [ ] No secrets in source code
- [ ] PII minimized in logs

### Infrastructure
- [ ] Dependencies scanned for CVEs
- [ ] Security headers configured
- [ ] Rate limiting in place

### Reviewed by: ___
### Date: ___
```

---

## Anti-Patterns

### ❌ "Ship Now, Secure Later"

Security debt compounds. Every insecure feature you ship is an open door. Fix it during development, not after a breach.

### ❌ "Claude Wrote It, So It's Fine"

AI-generated code needs the same (or more) scrutiny as human-written code. The speed of generation means more code to review, not less reviewing.

### ❌ Security Through Obscurity

```
Don't:  Hide the admin panel at /secret-admin-path
Do:     Require proper authentication and authorization
```

### ❌ Client-Side Only Validation

```
Don't:  Validate only in the browser (easily bypassed)
Do:     Validate on the server, with client validation as UX bonus
```

---

## Pro Tips

### 1. Review in Small Chunks

Don't dump 500 lines for review. Review each module or endpoint individually for better coverage.

### 2. Use Different Prompting Angles

```
# First pass: attacker perspective
"How would you exploit this?"

# Second pass: compliance perspective  
"Does this meet SOC2/GDPR requirements?"

# Third pass: operational perspective
"What could go wrong in production?"
```

### 3. Keep a Vulnerability Pattern Library

When you find a vulnerability pattern, document it:
```markdown
## Pattern: Missing rate limiting on auth endpoints
- Risk: Brute force attacks
- Detection: Check for express-rate-limit on login/register
- Fix: Add rate limiting middleware
- Test: Send 100 rapid requests, verify 429 after limit
```

### 4. Automate What You Can

Use the security review as input for automated checks:
```
Based on the vulnerabilities found, create:
1. ESLint rules to catch these patterns
2. Unit tests that verify the fixes
3. CI pipeline checks for regression
```

---

*Build fast, but build safe. The best vulnerability is the one that never reaches production.*
