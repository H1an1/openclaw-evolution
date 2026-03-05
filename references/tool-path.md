# 🔧 Tool Path — Building an Efficient System

You want your agent to get things done. Automate the boring stuff. Be proactive without being annoying. Here's how, level by level.

---

## Level 1: Messenger

**Where you are:** Agent can talk to you. That's it.

**What to do:**
- Complete the [Three Files](three-files-guide.md) setup
- Have a few conversations. Get a feel for how it responds.
- Adjust SOUL.md based on what you like/dislike

**You're ready for Level 2 when:** You've used it for 2-3 days and the conversation style feels right.

---

## Level 2: Secretary

**Goal:** Agent remembers things and keeps you organized.

### Memory System

Set up the basic memory structure:

```
workspace/
├── memory/
│   ├── 2026-03-01.md    ← Daily notes
│   ├── 2026-03-02.md
│   └── ...
├── MEMORY.md             ← Long-term (curated highlights)
└── NOW.md                ← Current state (read first after restart)
```

**Tell your agent** (in AGENTS.md):
```markdown
## Memory Rules
- Write daily notes to memory/YYYY-MM-DD.md
- Record: decisions made, tasks completed, things to remember
- Update NOW.md with current focus and next actions
- Weekly: consolidate important items into MEMORY.md
```

### Schedule Awareness

If you use a calendar or schedule file:
```markdown
## Schedule
- My schedule file: [path to your schedule]
- Read it each morning
- Remind me of upcoming events
```

**You're ready for Level 3 when:** Your agent consistently remembers yesterday's context and reminds you of things without being asked.

---

## Level 3: Operator

**Goal:** Agent does things on a schedule, checks on things, alerts you.

### Heartbeat

Heartbeat = periodic wake-up. Agent checks a list and acts.

Create `HEARTBEAT.md`:
```markdown
# HEARTBEAT.md
## Every heartbeat
1. Check if there are unread emails (important ones only)
2. Check calendar for events in next 2 hours
3. Update NOW.md

## Don't disturb
- 23:00-08:00 unless urgent
- When I'm clearly busy
```

### Cron Jobs

For precise timing, use cron:

```bash
# Morning schedule reminder at 8 AM
openclaw cron add --name "morning-schedule" \
  --cron "0 8 * * *" --tz "Asia/Shanghai" \
  --message "Read today's schedule and remind me of priorities" \
  --announce --channel telegram

# One-time reminder
openclaw cron add --name "dentist-reminder" \
  --at "2026-03-15T09:00:00" --tz "Asia/Shanghai" \
  --message "Remind me: dentist appointment at 10:00" \
  --announce --channel telegram --delete-after-run
```

**When to use which:**
| Heartbeat | Cron |
|-----------|------|
| Multiple checks batched together | Exact timing matters |
| Needs conversation context | Standalone task |
| Timing can drift ±30min | Must fire at 9:00 sharp |
| 2-4x daily checks | Specific schedule/one-shot |

### Skills

Install skills as you need them. See `references/essential-skills.md` for a tiered recommendation of what to install first.

```bash
# See what's available
openclaw skills list

# Install the essentials (Tier 1)
openclaw skills install weather
openclaw skills install github
openclaw skills install summarize
```

Start with Tier 1 skills. Add Tier 2 based on your workflow. Don't install everything — each skill adds to context.

**You're ready for Level 4 when:** Your agent proactively handles routine tasks and you trust it to act on its own for low-risk operations.

---

## Level 4: Orchestrator

**Goal:** Multiple agents working together, complex workflows.

### Multi-Agent Setup

When one agent isn't enough. See `references/multi-agent.md` for the complete architecture guide (single gateway vs multi-gateway, with pros/cons and real examples).

Two approaches:
- **Single Gateway** — All agents in one config. Simple, shared resources. Best for personal use.
- **Multiple Gateways** — Fully isolated instances. Best for different machines, different people, or production setups.

Common patterns:
- **Specialized agents** — One for coding, one for research, one for social media
- **Manager + worker pattern** — Main agent delegates to sub-agents
- **Different models** — Use cheaper models for simple tasks, powerful ones for complex work

### Workflow Automation

Combine cron + skills + multi-channel:

```
8:00 AM  → Agent reads schedule, sends Telegram summary
9:00 AM  → Agent checks GitHub PRs, posts updates to Discord
12:00 PM → Agent checks email, flags urgent items
6:00 PM  → Agent generates daily summary
```

### Advanced Skills

At this level, consider:
- Custom skills for your specific workflows
- Browser automation for web tasks
- API integrations via MCP servers

---

## Tool Path Principles

1. **Automate gradually** — Don't automate everything at once. Start with one recurring pain point.
2. **Trust but verify** — Let the agent do things, but review the results initially.
3. **Fail safely** — Configure safety rules so mistakes are recoverable (trash > rm).
4. **Batch similar tasks** — Group related checks into one heartbeat instead of many cron jobs.
5. **Measure value** — If an automation saves you less time than it takes to maintain, remove it.
