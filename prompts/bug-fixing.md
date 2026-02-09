# 🐛 Bug Fixing Prompts

Turn debugging from hours to minutes with these prompts.

---

## The Universal Bug Report

```
I'm getting [ERROR/BEHAVIOR] when [ACTION].

Expected: [what should happen]
Actual: [what's happening]

Error message:
```
[paste full error]
```

Relevant code:
```[language]
[paste relevant code]
```

What I've tried:
- [attempt 1]
- [attempt 2]
```

---

## Bug Categories

### 🔴 Runtime Error

```
Getting this error at runtime:

```
[full error with stack trace]
```

This happens when [describe trigger].

Here's the relevant code:
```[language]
[code]
```

What's causing this and how do I fix it?
```

### 🟡 Logic Bug

```
My code runs without errors but produces wrong results.

Input: [example input]
Expected output: [what it should produce]
Actual output: [what it produces]

Here's the logic:
```[language]
[code]
```

Walk me through what's happening step by step.
```

### 🟠 Performance Bug

```
This code is too slow:

```[language]
[code]
```

Current performance: [metrics]
Target performance: [goal]
Data size: [volume]

Profile/identify the bottleneck and suggest optimizations.
```

### 🔵 Intermittent Bug

```
I have a flaky bug that happens sometimes:

Symptoms: [describe]
Frequency: [how often]
Conditions: [when it seems to happen]

Suspected areas:
```[language]
[relevant code]
```

What could cause intermittent behavior here?
```

### 🟣 Integration Bug

```
[Service A] and [Service B] aren't communicating correctly.

Service A sends: [what it sends]
Service B receives: [what it gets, if anything]

Service A code:
```[language]
[code]
```

Service B code:
```[language]
[code]
```

Config/environment details:
- [relevant config]
```

---

## Debugging Workflows

### The Rubber Duck Prompt

```
I'm stuck on a bug. Let me explain it to you and help me spot the issue:

What I'm trying to do: [goal]
How I'm doing it: [approach]
Where it breaks: [specific point]

[Then explain your code line by line]
```

### The Fresh Eyes Prompt

```
Review this code for bugs. I've been staring at it too long:

```[language]
[code]
```

It should: [expected behavior]
But it: [actual behavior]

Look for obvious mistakes I might be missing.
```

### The Systematic Debug

```
Help me debug this systematically:

Problem: [describe]
Code: [paste code]

Let's add strategic console.logs/print statements to trace the execution flow.
```

---

## Error-Specific Prompts

### TypeError / Null Reference

```
Getting "[exact error message]" on line [X].

The variable [name] is supposed to be [type] but seems to be [null/undefined/wrong type].

Here's how it's used:
```[language]
[code showing variable usage]
```

Where could it be getting set incorrectly?
```

### Async/Await Issues

```
I think I have a race condition or async issue:

```[language]
[async code]
```

Sometimes [behavior A], sometimes [behavior B].

Help me trace the async flow and fix the timing.
```

### Database/Query Issues

```
This query isn't returning what I expect:

Query:
```sql
[query]
```

Expected: [X rows matching Y criteria]
Actual: [what I get]

Schema context:
```sql
[relevant schema]
```

Sample data:
[example rows]
```

---

## After the Fix

### Understanding the Root Cause

```
The fix works. Now explain:
1. What was the root cause?
2. Why did my original code fail?
3. How can I prevent this type of bug in the future?
```

### Preventing Regression

```
Add a test case that would have caught this bug:

The bug: [description]
The fix: [what we changed]

Write a test that fails before the fix and passes after.
```

---

## Pro Tips

1. **Include the FULL error** - Stack traces have clues
2. **Show your attempts** - Saves time, avoids repeated suggestions
3. **Isolate the problem** - Minimal reproduction > full codebase
4. **Version matters** - Include framework/library versions
5. **Recent changes** - "This worked until I [changed X]" is gold
