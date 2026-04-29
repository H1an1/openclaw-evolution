# Quick Start: First 24 Hours / 快速开始：第一天

A no-bullshit guide to getting your OpenClaw agent up and running in one day.
一份不废话的指南，一天内让你的 agent 跑起来。

---

## Step 0: Install / 安装

```bash
# Install OpenClaw
npm install -g openclaw

# Start the gateway
openclaw gateway start

# Connect Telegram (most common)
openclaw channels add telegram
```

Follow the prompts. You'll need a Telegram bot token from @BotFather.

---

## Step 1: Create Your Workspace / 创建工作区

```bash
mkdir ~/my-agent && cd ~/my-agent
openclaw init
```

This creates the skeleton. Now make it yours:

---

## Step 2: Write Three Files (10 minutes) / 写三个文件（10 分钟）

### SOUL.md — the shortest one that matters

```markdown
# SOUL.md
- Name: [pick a name]
- Style: [how it talks — direct? warm? funny?]
- Don't: [things that annoy you]
- Do: [things you want]
```

That's it. 4 lines. You'll rewrite it in a week. Don't overthink V1.

### USER.md — tell it who you are

```markdown
# USER.md
- Name: [your name]
- Work: [what you do]
- Timezone: [your timezone]
- Languages: [what you speak]
- One thing you hate: [e.g. "don't explain things I already know"]
```

### AGENTS.md — basic rules

```markdown
# AGENTS.md

## Every Session
1. Read SOUL.md
2. Read USER.md
3. Check memory/ folder

## Memory
- Write notes to memory/YYYY-MM-DD.md
- If something important happens, write it down

## Safety
- Ask before deleting anything
- Ask before sending public messages
- trash > rm
```

---

## Step 3: Talk to It / 开始对话

Send a message on Telegram. That's it. You now have an agent.

The first few conversations will feel generic. That's normal — it hasn't built context yet.

**Your job for Day 1:** Have 5-10 real conversations. Correct it when the style is wrong. Say "don't talk like that" or "shorter" or "more casual." It learns from the conversation history.

---

## Step 4: First Memory / 第一次记忆

After a few conversations, tell your agent:

> "记住这个"

or

> "Write this to memory"

Watch it create `memory/2026-xx-xx.md`. Now it won't forget tomorrow.

---

## What Happens Next / 接下来

- **Day 2-3:** Edit SOUL.md based on what annoyed you. Add rules to AGENTS.md.
- **Day 4-7:** Set up heartbeat (periodic check-ins). Add cron for reminders.
- **Week 2:** Add tools — calendar, email, web search. See [Tool Path](tool-path.md).
- **Week 3+:** Your agent starts feeling like "yours." The iteration loop is working.

---

## Common Day-1 Mistakes / 第一天常犯的错

| Mistake | Fix |
|---------|-----|
| Writing a 500-line SOUL.md | Start with 5 lines. Iterate. |
| Treating it like ChatGPT | Give it context about YOU. That's the whole point. |
| Not correcting bad behavior | Say "不要这样说话" immediately. Don't tolerate. |
| Expecting magic on Day 1 | It takes 3-5 days to feel different from ChatGPT. |
| Not writing memory | If the agent doesn't write notes, remind it. Memory = personality over time. |

---

## One-Line Summary

**Your agent becomes "yours" through iteration, not configuration. Write V1 of everything, use it for 3 days, then rewrite. Repeat forever.**

你的 agent 是迭代出来的，不是配置出来的。写 V1，用三天，然后重写。一直循环下去。
