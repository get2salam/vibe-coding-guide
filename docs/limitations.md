# ⚠️ When NOT to Vibe Code

Vibe coding is powerful, but it's not always the right tool. Know the limits.

---

## Red Flags 🚩

### 1. Security-Critical Code

**Don't vibe:**
- Authentication/authorization logic
- Cryptography implementations
- Payment processing
- Access control systems
- Sensitive data handling

**Why:** Security code needs deep understanding, not just working code. A subtle bug can be catastrophic.

**Instead:**
- Use established libraries (bcrypt, jose, stripe-sdk)
- Have security experts review
- Follow OWASP guidelines
- Vibe the scaffolding, hand-craft the security

### 2. Regulatory/Compliance Code

**Don't vibe:**
- HIPAA data handling
- GDPR consent management
- Financial reporting
- Audit logging
- Legal document generation

**Why:** Compliance requirements are precise. "It works" isn't enough—it must be provably correct.

**Instead:**
- Understand requirements deeply first
- Document every decision
- Have compliance review the code

### 3. High-Stakes Business Logic

**Don't vibe:**
- Billing calculations
- Financial transactions
- Medical dosage calculations
- Safety-critical systems
- Legal contract logic

**Why:** These need to be demonstrably correct. You need to understand every line.

**Instead:**
- Write these manually with tests
- Have domain experts review
- Use formal verification where possible

### 4. Performance-Critical Hot Paths

**Don't vibe:**
- High-frequency trading logic
- Real-time system kernels
- Game engine loops
- Database query optimizers
- Compression algorithms

**Why:** Performance optimization requires understanding hardware, memory, and profiling. AI-generated code is rarely optimal.

**Instead:**
- Profile first
- Understand the bottleneck
- Hand-optimize critical paths
- Vibe the non-critical parts

---

## Yellow Flags ⚡

### 1. Complex State Management

**Be careful with:**
- Distributed system state
- Cache invalidation
- Eventual consistency
- Race conditions
- Deadlock-prone code

**Why:** State bugs are subtle and hard to catch in review. AI might generate code that works "usually."

**Approach:**
- Vibe a first draft
- Think hard about edge cases
- Add extensive tests
- Consider formal modeling

### 2. Legacy System Integration

**Be careful with:**
- Undocumented APIs
- Fragile existing code
- Unusual conventions
- Version-specific behavior

**Why:** AI doesn't know your legacy system's quirks. It'll generate "standard" code that breaks.

**Approach:**
- Provide extensive context
- Show real examples from the codebase
- Test thoroughly in staging
- Be ready to iterate

### 3. Unusual Requirements

**Be careful with:**
- Niche domains (biotech, aerospace, etc.)
- Unusual architectures
- Proprietary protocols
- Cutting-edge tech (< 6 months old)

**Why:** AI training has less data on unusual things. Output may be wrong or outdated.

**Approach:**
- Verify against official docs
- Don't trust unusual patterns
- Cross-reference multiple sources

---

## Vibe Coding Limitations

### What Claude Doesn't Know

1. **Your infrastructure** - Server configs, network topology, deployment
2. **Your business rules** - Domain-specific logic, edge cases
3. **Your users** - What they actually do, not what you assume
4. **Recent changes** - Libraries updated after training cutoff
5. **Your preferences** - Until you tell it

### What Claude Can Get Wrong

1. **API details** - Parameter names, return types, versions
2. **Library specifics** - Methods that don't exist or changed
3. **Best practices** - Sometimes outdated or cargo-culted
4. **Error handling** - Often missing edge cases
5. **Security** - May miss subtle vulnerabilities

### What Claude Can't Do

1. **Run your code** - Can't verify it actually works
2. **See your screen** - Can't debug visual issues
3. **Access your systems** - Can't check your database, logs, etc.
4. **Know your context** - Unless you provide it
5. **Make value judgments** - Can't know what's "right" for your business

---

## When to Slow Down

### Before Production

Always manually review:
- Database migrations
- Destructive operations
- External API integrations
- Permission changes
- Data transformations

### Before Merging

Always verify:
- Tests pass (don't just generate tests—run them)
- Code does what you intended
- Edge cases are handled
- Error messages are useful
- Logging is appropriate

### Before Sharing

Always check:
- No secrets in code
- No personal data
- License compatibility
- Security implications

---

## The Verification Checklist

Before shipping vibe-coded code:

- [ ] I understand what this code does
- [ ] I've traced the main execution paths
- [ ] I've thought about failure modes
- [ ] I've run it with real(ish) data
- [ ] I've reviewed the error handling
- [ ] I've checked for security issues
- [ ] I've verified the tests are meaningful
- [ ] I could explain this code to someone else

---

## The Balance

**Vibe code:**
- Boilerplate and scaffolding
- Standard CRUD operations
- UI components
- Utility functions
- Test generation
- Documentation
- Configuration files
- Prototypes and MVPs

**Hand code:**
- Core business logic
- Security-sensitive code
- Performance-critical paths
- Unusual requirements
- Integration with legacy systems
- Regulatory compliance

**Hybrid approach:**
- Vibe the structure, hand-code the details
- Vibe the first draft, manually refine
- Vibe with heavy review for anything important

---

## Remember

> "Move fast and break things" doesn't apply when the things are:
> - Other people's money
> - Other people's data
> - Other people's health
> - Other people's safety

Vibe coding is a tool. Like any tool, knowing when NOT to use it is as important as knowing how to use it.

Ship fast, but ship responsibly.
