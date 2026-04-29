# Memory System Guide / 记忆系统指南

Your agent wakes up empty every session. Memory files are how it stays "itself."
你的 agent 每次醒来都是空的。记忆文件是它保持"自己"的方式。

---

## Architecture / 架构

```
workspace/
├── MEMORY.md          ← Long-term (curated insights, < 200 lines)
├── NOW.md             ← Current state (what am I doing RIGHT NOW)
└── memory/
    ├── 2026-04-28.md  ← Daily log (raw events)
    ├── 2026-04-29.md
    └── ...
```

### Three Layers / 三层

| Layer | Purpose | When to write | When to read |
|-------|---------|---------------|--------------|
| `memory/YYYY-MM-DD.md` | Daily log | During/after events | Start of session |
| `MEMORY.md` | Curated long-term | Weekly review | Every session |
| `NOW.md` | State lifeboat | During active work | After restart/compaction |

---

## Daily Log / 日记

The simplest and most important habit. Your agent should write to `memory/YYYY-MM-DD.md` whenever something worth remembering happens.

```markdown
# 2026-04-29

## Morning
- Yi submitted Red Dot application (Entry PD26335707)
- Cost: 600 SGD, category: Service Design

## Evening
- Dinner with Vivien + investor. Key insight: user data is the moat
- Antenna reached 200 users (doubled in one week)
```

**Rules:**
- One file per day
- Short entries, not essays
- Include decisions, events, and lessons
- Mark importance: `[P0]` permanent, `[P1]` archive after 30d, `[P2]` delete after 7d

---

## MEMORY.md / 长期记忆

Think of daily logs as raw notes. MEMORY.md is the distilled wisdom.

```markdown
# MEMORY.md

## About Yi
- Product designer, values directness
- Hates empty reassurance
- Timezone: Asia/Shanghai

## Key Decisions
- O-1 visa path chosen over H1B (2026-04-24)
- Antenna: agent-mediated social discovery (live since 2026-03)

## Lessons
- Don't send standalone emoji as messages
- Ask before destructive commands
- When Yi is in a meeting, don't interrupt
```

**Maintenance:**
- Review weekly (during heartbeat or dedicated session)
- Remove outdated info
- Keep under 200 lines — if it's growing, you're not curating enough

---

## NOW.md / 状态快照

Your "lifeboat" after context compression. When the conversation gets too long and compacts, NOW.md tells the next instance exactly where you left off.

```markdown
# NOW.md

## Current Focus
- Red Dot submission (deadline today 4/29)
- CCV investor call tonight

## Open Threads
- hermes-a2a PR #1 — waiting for review
- Kai gateway fixed, monitoring

## Next Actions
- Prepare investor pitch materials
- Update openclaw-evolution repo
```

**Update NOW.md:**
- Before going to sleep (if you're the agent)
- Before expected compaction
- When switching major tasks
- Think: "If I restart in 5 seconds, what do I need to know?"

---

## Search / 搜索记忆

When you need to recall something:

```
# Built-in
memory_search("keyword or question")

# For more precision, use fts (full-text search) on memory files
grep -r "keyword" memory/
```

---

## Anti-Patterns / 反模式

| ❌ Don't | ✅ Do |
|----------|------|
| Write everything to MEMORY.md | Daily → `memory/`, curate → MEMORY.md |
| Never delete old entries | Mark [P2], clean after 7 days |
| Rely on "mental notes" | Write it to a file. Mental notes don't survive restarts. |
| Write novels in daily log | 2-3 sentences per event. Context, not story. |
| Forget NOW.md | Update before any expected restart |
