# Agentic Workflow Pattern

## When to Use
- Multi-step tasks that require planning, execution, and verification
- Autonomous coding agents (Claude Code, Cursor, Copilot Workspace)
- Background task orchestration with cron/scheduled agents
- Self-healing systems that detect and fix issues automatically

## The Pattern

### 1. Observe → Plan → Execute → Verify

```
Human: "Add user authentication to the API"

Agent thinking:
1. OBSERVE: Read existing codebase, find auth-related files
2. PLAN: Need JWT middleware, user model, login/register endpoints, tests
3. EXECUTE: Write code file by file, run linter after each
4. VERIFY: Run tests, check for security issues, validate against requirements
```

### 2. Prompt Structure for Agentic Tasks

```markdown
## Role
You are a [specific role] agent. Your job: [one-line mission].

## Rules
1. [Most important constraint]
2. [Second constraint]
3. DO the work, don't just plan it

## Steps
1. [First action with exact command]
2. [Second action]
3. [Verification step]

## Output
[Exact format expected]

## Guardrails
- Do NOT [dangerous action]
- If [error condition], then [recovery action]
```

### 3. Self-Healing Agent Pattern

The agent detects failures and fixes them without human intervention:

```python
# Pseudo-code for a self-healing agent
def agent_loop():
    while True:
        status = check_system_health()
        
        if status.service_down:
            restart_service(status.service_name)
            notify("Restarted {service}")
            
        if status.scraper_dead and status.work_remaining:
            restart_scraper(status.last_checkpoint)
            notify("Scraper restarted from checkpoint")
            
        if status.all_healthy:
            return "HEARTBEAT_OK"  # Silent when everything's fine
        
        sleep(interval)
```

### 4. Multi-Agent Orchestration

Different agents with different specialties, coordinated by schedule:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Auditor   │     │   Builder   │     │  Watchdog   │
│  (Daily 6AM)│     │ (Daily 8PM) │     │(Every 30min)│
│             │     │             │     │             │
│ Reads data  │────▶│ Reads audit │     │ Checks all  │
│ Finds issues│     │ Fixes #1    │     │ Restarts if │
│ Grades A-F  │     │ Ships code  │     │ dead        │
└─────────────┘     └─────────────┘     └─────────────┘
```

**Key principles:**
- Each agent has ONE job
- Agents communicate via files/APIs, not direct messaging
- Schedule agents to avoid resource conflicts (e.g., only one PLS session at a time)
- Watchdog pattern: silent when healthy, loud when broken

### 5. Conflict Prevention

When multiple agents access shared resources:

```markdown
## Before accessing shared resource:
1. Check if another agent is using it
2. If busy → back off and retry later
3. If free → acquire and proceed
4. Always release when done
```

Example for database/API access:
```bash
# Check for active scrapers before running audit
Get-Process python | Where-Object { $_.CommandLine -match 'scraper' }
# If any found → "Scraper active, audit postponed"
# If none → proceed with audit
```

## Anti-Patterns

### ❌ God Agent
One agent that does everything. Breaks constantly, impossible to debug.

**Fix:** Split into specialist agents with clear boundaries.

### ❌ Chatty Agents
Agents that notify on every heartbeat. User disables notifications.

**Fix:** Silent when healthy (HEARTBEAT_OK). Only notify on issues or completions.

### ❌ No Checkpointing
Agent crashes mid-task, loses all progress, starts over.

**Fix:** Save progress after each step. Resume from last checkpoint.

### ❌ Resource Contention
Two agents hit the same API simultaneously, both fail.

**Fix:** Schedule non-overlapping time slots. Add resource checks before acquiring.

## Real-World Example: Legal Data Platform

A production system with 24 agents:

| Category | Agents | Purpose |
|----------|--------|---------|
| Scraping | 3 | Discover reporters, scrape cases, fill gaps |
| Auditing | 4 | Case law audit, legislation audit, court audit, gap detective |
| Building | 3 | GitHub commits, LexiSearch features, dashboard improvements |
| Monitoring | 3 | Watchdog (services), hourly status, CEO review |
| Operations | 3 | Dashboard update, memory save, platform improvement |

**Total API calls saved by bundling:** 18 individual calls → 1 single `/dashboard` endpoint.

**Conflict prevention:** All PLS-touching agents check for active scrapers before running.

## Prompting Tips for Agents

1. **Be explicit about output format** — agents will improvise if you don't specify
2. **Include exact commands** — don't say "run tests", say `python -m pytest tests/ -q`
3. **Add escape hatches** — "If login fails, report and EXIT. Do NOT retry aggressively."
4. **Specify what NOT to do** — "Do NOT call the message tool" prevents duplicate delivery
5. **One task per agent** — "Build ONE feature. Make it count." beats "Build 3-5 features"
