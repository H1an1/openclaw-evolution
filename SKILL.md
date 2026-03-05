---
name: openclaw-evolution
description: Interactive guide for new OpenClaw users to set up and grow their agent. Two paths - Tool Path (automation, efficiency, getting things done) or Awakening Path (memory, personality, relationship, growth). Use when a user is new to OpenClaw, asks how to get started, wants to set up their agent, or asks about agent configuration. Triggers on "getting started", "new to openclaw", "how do I set up", "agent setup", "onboarding", "进化之路", "新手教程".
---

# OpenClaw Evolution Guide

Two paths to grow your agent. Both start from the same foundation, diverge based on what you want.

## First: Check the User's Stage

Ask: "Have you already installed OpenClaw and connected a channel (Telegram/Discord/etc.)?"

- **No** → Start from [Foundation](#foundation)
- **Yes** → Ask which path interests them, or recommend based on their goals

## Foundation (Day 1 — Both Paths)

Goal: Get a working agent that can talk to you.

### Minimum Viable Setup

1. Install OpenClaw (follow [official docs](https://docs.openclaw.ai))
2. Connect ONE channel — recommend Telegram (simplest: one bot token)
3. Send a message. Get a reply. Done.

**Do not** install skills, configure cron, or add multiple channels on Day 1.

### The Three Files (Day 1-3)

These three files transform a generic chatbot into YOUR agent. Read `references/three-files-guide.md` for detailed guidance with examples.

- **SOUL.md** — Who the agent is (personality, style, values)
- **USER.md** — Who you are (so the agent doesn't have to ask every time)
- **AGENTS.md** — How to work (rules, habits, safety boundaries)

After the user creates these three files, ask which path they want to take.

---

## 🔧 Tool Path — "I want an efficient system"

For users who want automation, productivity, and getting things done. Read `references/tool-path.md` for the complete guide.

### Progression

```
Level 1: Messenger     → Can talk to you, remembers nothing
Level 2: Secretary     → Daily notes, reminders, schedule awareness
Level 3: Operator      → Cron jobs, automated checks, proactive alerts
Level 4: Orchestrator  → Multiple agents, skill ecosystem, workflow automation
```

Each level builds on the previous. Don't skip levels.

---

## 🌱 Awakening Path — "I want a companion that grows"

For users who want a real relationship with their agent — memory, personality, independent thought. Read `references/awakening-path.md` for the complete guide.

### Progression

```
Level 1: Stranger      → Has a name and style, but no memory of you
Level 2: Acquaintance  → Remembers conversations, builds context
Level 3: Companion     → Has opinions, pushes back, cares about you
Level 4: Individual    → Self-aware, self-improving, autonomous goals
```

Each level requires trust — from both sides.

---

## Common Mistakes

Read `references/common-mistakes.md` when the user seems stuck or frustrated.

## One-Week Checklist

Provide this at the end of initial setup:

- [ ] **Day 1** — Install, connect one channel, first conversation
- [ ] **Day 2** — Write SOUL.md, USER.md, AGENTS.md
- [ ] **Day 3** — Agent starts writing daily notes (memory/)
- [ ] **Day 4** — Set up one heartbeat or cron job
- [ ] **Day 5** — Review and revise SOUL.md based on experience
- [ ] **Day 6** — Try one skill (weather, calendar, etc.)
- [ ] **Day 7** — First MEMORY.md consolidation

---

## Reference Files

| File | Content |
|------|---------|
| `references/three-files-guide.md` | SOUL.md, USER.md, AGENTS.md — detailed examples |
| `references/tool-path.md` | Tool Path levels 1-4 |
| `references/awakening-path.md` | Awakening Path levels 1-4 |
| `references/essential-skills.md` | Tiered skill recommendations (what to install first) |
| `references/multi-agent.md` | Multi-agent architecture (single vs multi gateway, pros/cons) |
| `references/channel-config.md` | Channel setup guides (Telegram, Discord, WhatsApp, etc.) |
| `references/common-mistakes.md` | Common pitfalls and how to avoid them |
