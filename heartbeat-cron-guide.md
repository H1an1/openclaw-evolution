# Heartbeat & Cron Guide / 心跳与定时任务

Make your agent proactive instead of reactive.
让你的 agent 主动出击而不是被动应答。

---

## Heartbeat / 心跳

A heartbeat is a periodic ping that wakes your agent up to check if anything needs attention.

### Setup / 配置

In your OpenClaw config (`openclaw.json`):

```json
{
  "agents": {
    "list": [{
      "id": "main",
      "heartbeat": {
        "every": "30m",
        "prompt": "Check if anything needs attention. If nothing, reply HEARTBEAT_OK."
      }
    }]
  }
}
```

### What to Check / 检查什么

Create `HEARTBEAT.md` in your workspace with a checklist:

```markdown
# HEARTBEAT.md

## Check these (pick 1-2 per heartbeat, rotate):
- [ ] Unread emails?
- [ ] Calendar — anything in next 2 hours?
- [ ] GPS — did user move somewhere new?
- [ ] Pending tasks from last conversation?

## Rules:
- Don't disturb 23:00-08:00
- If nothing needs attention → HEARTBEAT_OK
- If something matters → send a message
```

### Good Heartbeat Behavior / 好的心跳行为

```
✅ "You have a meeting in 30 minutes"
✅ "Important email from X just arrived"
✅ HEARTBEAT_OK (when nothing's happening)

❌ "Just checking in! How are you?" (every 30 minutes)
❌ Repeating yesterday's reminders
❌ Waking user at 3 AM for non-urgent things
```

---

## Cron / 定时任务

For tasks that need exact timing or isolation from main conversation.

### Setup / 配置

```bash
openclaw cron add \
  --name "morning-schedule" \
  --schedule "0 8 * * *" \
  --timezone "Asia/Shanghai" \
  --prompt "Read today's schedule file and send Yi a morning briefing"
```

### When to Use Cron vs Heartbeat

| Use Cron | Use Heartbeat |
|----------|---------------|
| Exact time matters (8:00 AM sharp) | Timing can drift (every ~30 min) |
| Isolated from main session | Needs recent conversation context |
| One-shot delivery | Multiple checks batched together |
| Different model/thinking level | Same session, same context |

### Example Cron Jobs / 示例

```bash
# Morning schedule reminder
openclaw cron add --name "morning" --schedule "0 8 * * *" \
  --prompt "Read today's schedule, remind Yi of priorities"

# Sleep reminder
openclaw cron add --name "sleep" --schedule "30 22 * * *" \
  --prompt "Gently remind Yi to sleep"

# Weekly review
openclaw cron add --name "weekly" --schedule "0 10 * * 0" \
  --prompt "Review this week's memory files, summarize key events"

# Daily backup
openclaw cron add --name "backup" --schedule "0 5 * * *" \
  --prompt "Git commit and push workspace changes"
```

### Managing Crons / 管理

```bash
openclaw cron list          # See all crons
openclaw cron delete <id>   # Remove one
openclaw cron disable <id>  # Pause without deleting
```

---

## Pro Tips / 进阶技巧

1. **Start with 2-3 crons max.** Too many = notification fatigue.
2. **Heartbeat is for checking, not doing.** Heavy work → spawn a sub-agent or cron.
3. **Track what you checked** in a state file to avoid repeating:
   ```json
   // memory/heartbeat-state.json
   {"lastChecks": {"email": 1714300800, "calendar": 1714300800}}
   ```
4. **Night shift:** If your agent has creative work (writing, reading, thinking), schedule it for hours when you're asleep. It won't bother you, and gets "alone time."
