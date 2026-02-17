# 🔒 Security in Vibe Coding

AI-assisted development ships faster — but speed without security is a liability. This guide covers how to maintain strong security posture while vibe coding.

---

## Why Security Matters More with AI

When AI generates code, it optimizes for **functionality**. It makes things *work*. But:

- Generated code may use outdated patterns with known vulnerabilities
- AI might hardcode secrets in examples that get committed
- Default configurations are rarely production-secure
- Generated SQL, shell commands, or queries may be injectable
- Dependency suggestions might include packages with CVEs

**Your job**: Direct the vibes, but verify the security.

---

## The Security-First Vibe Workflow

### 1. Set Security Context Up Front

Always include security requirements in your initial prompt:

```
Build a user registration API.

Security requirements:
- Hash passwords with bcrypt (cost factor 12+)
- Validate and sanitize all inputs
- Rate limit registration to 5/min per IP
- No secrets in code — use environment variables
- Parameterized queries only (no string concatenation)
- Return generic error messages (don't leak internals)
```

### 2. Request Threat Modeling

Before implementation, ask Claude to think adversarially:

```
Before coding, list the top 10 security threats for this feature.
For each threat:
- Attack vector
- Impact (high/medium/low)
- Mitigation strategy

Then implement with all mitigations built in.
```

### 3. Security Review Pass

After implementation, run a dedicated security review:

```
Review this code for security vulnerabilities:

Check for:
- Injection attacks (SQL, XSS, command injection)
- Authentication/authorization bypasses
- Sensitive data exposure
- Missing input validation
- Insecure defaults
- Hardcoded secrets
- Missing rate limiting
- CORS misconfiguration

Be thorough. I'd rather fix it now than in production.
```

---

## Common Vulnerability Patterns in AI-Generated Code

### 🚨 SQL Injection

**AI often generates:**
```python
# VULNERABLE — string concatenation
query = f"SELECT * FROM users WHERE email = '{email}'"
cursor.execute(query)
```

**Always request:**
```python
# SAFE — parameterized query
query = "SELECT * FROM users WHERE email = %s"
cursor.execute(query, (email,))
```

**Prompt fix:**
```
Use parameterized queries everywhere. Never concatenate user input
into SQL strings. Show me every database query so I can verify.
```

### 🚨 Cross-Site Scripting (XSS)

**AI often generates:**
```javascript
// VULNERABLE — direct HTML injection
element.innerHTML = userInput;
```

**Always request:**
```javascript
// SAFE — text content or sanitized HTML
element.textContent = userInput;
// Or with a sanitizer
element.innerHTML = DOMPurify.sanitize(userInput);
```

**Prompt fix:**
```
Sanitize all user-provided content before rendering.
Use textContent for plain text, DOMPurify for HTML.
Never use innerHTML with unsanitized input.
```

### 🚨 Hardcoded Secrets

**AI often generates:**
```python
# VULNERABLE — secret in source code
API_KEY = "sk-abc123..."
db_password = "supersecret"
```

**Always request:**
```python
# SAFE — environment variables
import os
API_KEY = os.environ["API_KEY"]
db_password = os.environ["DB_PASSWORD"]
```

**Prompt fix:**
```
Never hardcode secrets, API keys, or passwords.
Use environment variables for all sensitive configuration.
Add a .env.example with placeholder values (not real secrets).
Ensure .env is in .gitignore.
```

### 🚨 Insecure Authentication

**AI often generates:**
```python
# VULNERABLE — weak hashing
password_hash = hashlib.md5(password.encode()).hexdigest()
```

**Always request:**
```python
# SAFE — proper password hashing
import bcrypt
password_hash = bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))
```

**Prompt fix:**
```
Use bcrypt or argon2 for password hashing. Never MD5 or SHA for passwords.
Implement proper session management with secure, httpOnly cookies.
Add CSRF protection on all state-changing endpoints.
```

### 🚨 Missing Authorization Checks

**AI often generates:**
```python
# VULNERABLE — no authorization check
@app.route("/api/users/<user_id>", methods=["DELETE"])
def delete_user(user_id):
    db.users.delete(user_id)
    return {"status": "deleted"}
```

**Always request:**
```python
# SAFE — verify authorization
@app.route("/api/users/<user_id>", methods=["DELETE"])
@require_auth
def delete_user(user_id):
    current_user = get_current_user()
    if current_user.id != user_id and not current_user.is_admin:
        abort(403)
    db.users.delete(user_id)
    return {"status": "deleted"}
```

---

## Security Checklists

### Authentication & Session Management

- [ ] Passwords hashed with bcrypt/argon2 (cost factor ≥ 12)
- [ ] Session tokens are cryptographically random (≥ 256 bits)
- [ ] Sessions expire after inactivity (configurable timeout)
- [ ] Secure, HttpOnly, SameSite cookies for session tokens
- [ ] Login rate limiting (account lockout after N failures)
- [ ] Password complexity requirements enforced
- [ ] Secure password reset flow (time-limited tokens)

### Input Validation

- [ ] All user input validated on the server side
- [ ] Parameterized queries for all database operations
- [ ] Input length limits enforced
- [ ] File upload validation (type, size, content)
- [ ] URL/redirect validation (prevent open redirects)
- [ ] Content-Type verification on requests

### API Security

- [ ] Authentication required on all non-public endpoints
- [ ] Authorization checks on every resource access
- [ ] Rate limiting per endpoint and per user/IP
- [ ] CORS configured for specific allowed origins (not `*`)
- [ ] Request size limits configured
- [ ] Sensitive data excluded from API responses
- [ ] API versioning for breaking changes

### Data Protection

- [ ] Sensitive data encrypted at rest (AES-256)
- [ ] TLS 1.2+ enforced for data in transit
- [ ] PII minimized — only collect what's needed
- [ ] Database credentials in environment variables
- [ ] Secrets never logged or included in error messages
- [ ] Backups encrypted and access-controlled

### HTTP Security Headers

- [ ] `Content-Security-Policy` — restrict resource loading
- [ ] `X-Content-Type-Options: nosniff` — prevent MIME sniffing
- [ ] `X-Frame-Options: DENY` — prevent clickjacking
- [ ] `Strict-Transport-Security` — enforce HTTPS
- [ ] `X-XSS-Protection: 0` — rely on CSP instead
- [ ] `Referrer-Policy: strict-origin-when-cross-origin`

### Dependency Security

- [ ] `npm audit` / `pip audit` / `cargo audit` in CI
- [ ] Dependabot or Renovate configured for auto-updates
- [ ] Lock files committed (package-lock.json, Pipfile.lock)
- [ ] No wildcard version ranges in production dependencies
- [ ] License compliance checked

---

## Security Prompts for Common Scenarios

### Secure File Upload

```
Implement secure file upload:

- Validate file type by magic bytes (not just extension)
- Enforce max size (10MB)
- Generate random filenames (don't use original)
- Store outside web root
- Scan for malware if possible
- Serve through a controller, not direct file access
- Set Content-Disposition: attachment on downloads
```

### Secure API Key Management

```
Implement API key authentication:

- Generate keys with crypto.randomBytes(32)
- Store only the hash (SHA-256) in the database
- Show the key to the user exactly once at creation
- Support key rotation (create new, deprecate old)
- Include key prefix for identification (e.g., "sk_live_...")
- Log key usage (without logging the key itself)
- Rate limit per key
```

### Secure Error Handling

```
Implement error handling that:

- Returns generic messages to clients ("Something went wrong")
- Logs detailed errors server-side with stack traces
- Never exposes internal paths, query details, or stack traces to users
- Uses consistent error response format
- Includes correlation IDs for debugging
- Handles all error types (validation, auth, not found, server)
```

### Content Security Policy

```
Generate a Content-Security-Policy header for my app:

- App serves from example.com
- Uses Google Fonts
- Has inline styles (from a CSS-in-JS library)
- Makes API calls to api.example.com
- No inline scripts allowed
- No eval() allowed
- Report violations to /api/csp-report
```

---

## Automated Security in CI/CD

### GitHub Actions Security Workflow

```yaml
name: Security Checks

on: [push, pull_request]

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run dependency audit
        run: npm audit --audit-level=high

      - name: SAST scan
        uses: github/codeql-action/analyze@v3
        with:
          languages: javascript

      - name: Secret detection
        uses: trufflesecurity/trufflehog@main
        with:
          path: .
          base: ${{ github.event.repository.default_branch }}

      - name: Container scan
        if: hashFiles('Dockerfile') != ''
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: fs
          exit-code: 1
          severity: HIGH,CRITICAL
```

### Pre-Commit Security Hooks

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']

  - repo: https://github.com/PyCQA/bandit
    rev: 1.7.7
    hooks:
      - id: bandit
        args: ['-r', 'src/', '-ll']

  - repo: https://github.com/eslint/eslint
    rev: v8.56.0
    hooks:
      - id: eslint
        args: ['--ext', '.js,.ts', '--rule', 'no-eval: error']
```

---

## OWASP Top 10 Quick Reference

| # | Risk | Vibe Coding Mitigation |
|---|------|----------------------|
| A01 | Broken Access Control | "Add authorization checks on every endpoint" |
| A02 | Cryptographic Failures | "Use bcrypt for passwords, AES-256 for data at rest" |
| A03 | Injection | "Parameterized queries, input sanitization everywhere" |
| A04 | Insecure Design | "Threat model before implementing" |
| A05 | Security Misconfiguration | "Show me all default configs, harden each one" |
| A06 | Vulnerable Components | "Run dependency audit, pin versions" |
| A07 | Auth Failures | "Implement MFA, rate limit login, secure sessions" |
| A08 | Data Integrity Failures | "Verify signatures, validate CI/CD pipeline integrity" |
| A09 | Logging Failures | "Log security events, never log secrets" |
| A10 | SSRF | "Validate URLs, block internal network requests" |

---

## Pro Tips

### 1. Security as a Non-Functional Requirement

Always include security in your initial context:
```
AGENTS.md:
## Security Standards
- OWASP Top 10 mitigations required
- All inputs validated server-side
- Parameterized queries only
- No secrets in code
```

### 2. Assume Generated Code is Insecure

Review every generated snippet for:
- Hardcoded values that should be config
- Missing validation on inputs
- Overly permissive defaults
- Missing error handling that could leak info

### 3. Use Security Linters

Ask Claude to configure them for you:
```
Set up ESLint security rules, Bandit for Python,
and pre-commit hooks for secret detection.
```

### 4. Test Adversarially

```
Write tests that attempt to:
- SQL inject every input field
- XSS every text output
- Access resources without auth
- Exceed rate limits
- Upload malicious files
```

### 5. Keep a Security Decision Log

Document every security decision:
```markdown
## Security Decisions
| Date | Decision | Rationale |
|------|----------|-----------|
| 2025-01-15 | bcrypt cost 12 | Balance of security vs latency |
| 2025-01-15 | JWT in httpOnly cookie | Prevent XSS token theft |
| 2025-01-16 | Rate limit: 100/min | Prevent brute force |
```

---

*Security isn't a feature — it's a property of every feature. Vibe with confidence, ship with safety.*
