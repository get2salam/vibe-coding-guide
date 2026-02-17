# 🎸 Vibe Coding Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude](https://img.shields.io/badge/Powered%20by-Claude%20Opus%204-blueviolet)](https://anthropic.com)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Stars](https://img.shields.io/github/stars/get2salam/vibe-coding-guide?style=social)](https://github.com/get2salam/vibe-coding-guide)

> **"Coding by vibes"** — Describe what you want, let AI handle the implementation, iterate conversationally.

The definitive guide to **Vibe Coding** with Claude Code and Opus 4. Stop wrestling with syntax. Start shipping features.

---

## 🌊 What is Vibe Coding?

**Vibe Coding** is a development paradigm where you:

1. **Describe intent** in natural language
2. **Let AI implement** the solution
3. **Iterate conversationally** until it's perfect
4. **Ship faster** than ever before

```
Traditional: Think → Type → Debug → Google → Stack Overflow → Debug → Ship
Vibe Coding: Describe → Review → Ship
```

### The Philosophy

> "I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works."
> — Andrej Karpathy

Vibe coding isn't about being lazy. It's about **staying in flow**. You focus on *what* you're building while AI handles *how* to build it.

---

## 🚀 Quick Start

### 1. Set Up Claude Code

```bash
# Install Claude Code CLI
npm install -g @anthropic-ai/claude-code

# Authenticate
claude auth login

# Start vibing
claude
```

### 2. Your First Vibe

```
You: Build me a REST API for a todo app with user authentication

Claude: I'll create a complete todo API with JWT auth...
[implements full solution]

You: Add rate limiting and make the error messages friendlier

Claude: Done. I've added express-rate-limit and improved error handling...
```

### 3. Ship It

```bash
You: Deploy this to fly.io with a postgres database

Claude: Setting up fly.io deployment...
[configures everything, deploys]
```

---

## 📚 Guide Contents

### [📖 Prompts](./prompts/)
Battle-tested prompts for common development tasks:
- [Feature Development](./prompts/feature-development.md)
- [Bug Fixing](./prompts/bug-fixing.md)
- [Code Review](./prompts/code-review.md)
- [Refactoring](./prompts/refactoring.md)
- [Testing](./prompts/testing.md)
- [Documentation](./prompts/documentation.md)

### [🔒 Security](./docs/security.md)
Keep your AI-generated code secure:
- OWASP Top 10 mitigations for vibe coding
- Common vulnerability patterns in AI-generated code
- Security checklists for auth, APIs, data protection
- CI/CD security automation workflows

### [🔧 Workflows](./workflows/)
End-to-end workflows for real projects:
- [Greenfield Project](./workflows/greenfield.md)
- [Legacy Codebase](./workflows/legacy.md)
- [API Development](./workflows/api.md)
- [Frontend SPA](./workflows/frontend.md)
- [Full-Stack App](./workflows/fullstack.md)

### [💡 Patterns](./patterns/)
Design patterns that leverage Claude's strengths:
- [Iterative Refinement](./patterns/iterative-refinement.md)
- [Context Loading](./patterns/context-loading.md)
- [Multi-File Orchestration](./patterns/multi-file.md)
- [Test-Driven Vibes](./patterns/test-driven.md)
- [Security Review](./patterns/security-review.md)

### [🎯 Examples](./examples/)
Real-world examples with full conversations:
- [Building a CLI Tool](./examples/cli-tool.md)
- [React Dashboard](./examples/react-dashboard.md)
- [Python Data Pipeline](./examples/data-pipeline.md)
- [Chrome Extension](./examples/chrome-extension.md)

---

## 🎨 The Vibe Coding Mindset

### ✅ Do This

| Instead of... | Try... |
|--------------|--------|
| "Write a function that..." | "I need to [goal]. Handle edge cases." |
| Specifying implementation | Describing the outcome you want |
| Writing boilerplate | Asking Claude to scaffold it |
| Debugging line by line | Pasting the error and asking for fixes |
| Reading docs for hours | "How do I [thing] with [library]?" |

### ❌ Avoid This

- **Over-specifying**: Let Claude make reasonable decisions
- **Micro-managing**: Trust the implementation, review the result
- **Ignoring context**: Keep Claude informed about your project structure
- **Skipping review**: Always verify critical logic and [security](./docs/security.md)

---

## 🔥 Pro Tips

### 1. Start with Context

```
I'm building a [type] app using [stack].
The codebase structure is: [brief overview]
Current task: [what you need]
```

### 2. Iterate, Don't Restart

```
✅ "Actually, make it async and add retry logic"
❌ [Starting new conversation with full re-explanation]
```

### 3. Use AGENTS.md

Create an `AGENTS.md` file in your repo root:

```markdown
# AGENTS.md

## Project Context
This is a SaaS dashboard for analytics.
Stack: Next.js 14, Prisma, PostgreSQL, Tailwind

## Conventions
- Use server components by default
- All API routes in /app/api
- Zod for validation

## Current Sprint
- Building user settings page
- Implementing team invites
```

### 4. Let Claude Handle the Boring Stuff

- Boilerplate and scaffolding
- Type definitions
- Test setup
- Config files
- Documentation
- Migration scripts

### 5. Stay in the Loop for the Important Stuff

- Business logic decisions
- Security implementations
- Database schema design
- Architecture choices

---

## 📊 Vibe Coding vs Traditional Development

| Aspect | Traditional | Vibe Coding |
|--------|-------------|-------------|
| **Time to first feature** | Hours/Days | Minutes |
| **Boilerplate** | Manual | Generated |
| **Context switching** | High (IDE, docs, browser) | Low (conversational) |
| **Learning curve** | Steep | Gentle |
| **Code ownership** | You write it | You direct it |
| **Debugging** | Solo | Pair programming with AI |

---

## 🛠️ Recommended Setup

### Essential Tools

- **[Claude Code](https://docs.anthropic.com/claude-code)** - The CLI that makes vibe coding possible
- **[Cursor](https://cursor.sh)** - IDE with Claude integration
- **[OpenClaw](https://openclaw.dev)** - Agent framework for autonomous workflows

### Model Selection

| Use Case | Recommended Model |
|----------|------------------|
| Complex architecture | Claude Opus 4 |
| Quick iterations | Claude Sonnet 4 |
| Simple tasks | Claude Haiku |

### Project Setup

```bash
# Initialize with AGENTS.md
echo "# Project Context\n\nDescribe your project here." > AGENTS.md

# Add to .gitignore
echo ".claude/" >> .gitignore
```

---

## 🤝 Contributing

Vibe coding is evolving fast. Contributions welcome!

1. Fork the repo
2. Add your prompts/workflows/patterns
3. Submit a PR

### What We're Looking For

- 🎯 Battle-tested prompts that work reliably
- 📖 Real-world workflow documentation
- 💡 Novel patterns that leverage Claude's capabilities
- 🐛 Bug fixes and improvements

---

## 📖 Further Reading

- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [Anthropic's Claude](https://www.anthropic.com/claude)
- [The Art of Prompt Engineering](./docs/prompt-engineering.md)
- [Security in Vibe Coding](./docs/security.md)
- [When NOT to Vibe Code](./docs/limitations.md)

---

## 🌟 Star History

If this guide helped you, give it a ⭐!

---

## 📜 License

MIT License - Vibe freely.

---

<p align="center">
  <b>Built with 🎸 vibes by <a href="https://github.com/get2salam">Abdul Salam</a></b><br>
  <sub>Stop typing. Start vibing.</sub>
</p>
