# 🔄 Iterative Refinement Pattern

The core pattern of vibe coding: start rough, refine conversationally.

---

## The Pattern

```
1. Describe → Get initial implementation
2. Review → Identify what's not quite right
3. Refine → Ask for specific changes
4. Repeat → Until it's exactly what you need
```

---

## Why It Works

Traditional development:
```
Think → Plan → Type → Debug → Fix → Type more → Debug more
```

Iterative refinement:
```
Describe → Review → "Actually, change X" → Review → Ship
```

You stay in **design mode**, not **typing mode**. Claude handles the translation from intent to code.

---

## In Practice

### Starting Point

```
Build a function that validates email addresses.
```

**Claude produces:**
```javascript
function validateEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}
```

### First Refinement

```
Good start. Also:
- Return an object with { valid, reason }
- Check for common typos like "gmial.com"
- Reject disposable email domains
```

**Claude refines:**
```javascript
const disposableDomains = ['tempmail.com', 'throwaway.com', ...];
const typoFixes = { 'gmial.com': 'gmail.com', ... };

function validateEmail(email) {
  // ... enhanced implementation
  return { valid: true, reason: null };
}
```

### Second Refinement

```
Nice. Now:
- Make disposable domains configurable
- Add TypeScript types
- Export as named export
```

### Final Refinement

```
Perfect. Add JSDoc comments and a few unit tests.
```

---

## Refinement Prompts

### Additive Refinements

```
Also add [feature]
```

```
Include handling for [edge case]
```

```
Add [logging/tests/types/docs]
```

### Corrective Refinements

```
Actually, change [X] to [Y]
```

```
That's not quite right - [explanation of what's wrong]
```

```
This should [expected behavior] but it [actual behavior]
```

### Subtractive Refinements

```
Remove [unnecessary thing]
```

```
Simplify this - we don't need [feature]
```

```
Too complex - just do the basic case for now
```

### Style Refinements

```
Make this more [readable/idiomatic/concise]
```

```
Follow [convention/pattern] instead
```

```
Match the style of [example code]
```

---

## Anti-Patterns

### ❌ Starting Over

```
[After several refinements]
"Actually let's start fresh..."
```

You lose all the refinements. Better to say:
```
"Keep everything but restructure [specific part]"
```

### ❌ Vague Refinements

```
"Make it better"
"This doesn't feel right"
```

Better:
```
"Improve performance by caching [X]"
"The naming is confusing - rename [A] to [B]"
```

### ❌ Too Many Changes at Once

```
"Change A, B, C, D, E, and F"
```

Better to batch related changes:
```
"First, let's fix the validation: [A, B]"
[Review]
"Now the error handling: [C, D]"
```

---

## Pro Tips

### 1. Start Broad, Narrow Down

```
Round 1: "Build X"
Round 2: "Handle edge cases Y and Z"
Round 3: "Optimize the query"
Round 4: "Add logging"
```

### 2. Name Your Constraints

```
"Keep the public API the same"
"Don't change [specific file]"
"Must be backwards compatible"
```

### 3. Reference Previous Context

```
"Like you did with the user validation"
"Same pattern as the auth middleware"
"Following the approach from earlier"
```

### 4. Know When to Stop

Good enough for MVP beats perfect but never shipped.

```
"This is good enough for now. We can optimize later if [condition]"
```

---

## The Golden Rule

Each refinement should be:
- **Specific** - Claude knows exactly what to change
- **Bounded** - One concern at a time
- **Additive** - Build on what's working

Think of it like sculpting: start with the rough shape, then refine the details.
