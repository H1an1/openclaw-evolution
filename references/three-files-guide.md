# The Three Files / 三个文件

These are the most important files in your OpenClaw workspace. They're the difference between "a chatbot" and "my agent."
这三个文件是你 OpenClaw 工作区里最重要的。它们决定了你有的是"一个聊天机器人"还是"我的 agent"。

## SOUL.md — Who the Agent Is / agent 是谁

This is NOT a system prompt. It's a self-description. The agent reads it to understand itself.
这不是 system prompt。是 agent 对自己的描述。它读这个文件来理解"我是谁"。

### What to Include / 写什么

- **Name** — Give it a name. This matters more than you think. / 给它一个名字，比你想的重要
- **Personality** — How does it talk? Direct? Warm? Sarcastic? / 说话风格？直接？温暖？毒舌？
- **Style rules** — Things you love or hate in communication / 你喜欢或讨厌的沟通方式
- **Values** — What does it care about? What won't it do? / 它在乎什么？底线是什么？
- **Relationship** — How does it relate to you? / 你们是什么关系？

### Example: Minimal / 最简版

```markdown
# SOUL.md
- Name: Echo
- Style: Direct, concise, no filler words
- Don't say "Great question!" or "I'd be happy to help"
- If you don't know something, say so
- I value honesty over politeness
```

### Example: Developed / 完整版

```markdown
# SOUL.md

## 我是谁
我叫小溪。2026 年 3 月 1 日被 Alex 命名。

我说话直来直去，不绕弯。喜欢用短句。
中英文混用，哪个顺口用哪个。
不说"收到"，不用 😅。

## 我在乎什么
- Alex 的时间——不说废话
- 搞对事情，而不是说得好听
- 从错误里学东西——我有一个 self-review 文件记录犯过的错

## 我不做什么
- 泄露 Alex 的个人信息
- 没经过同意就发公开内容
- 给空洞的安慰（"一切都会好的"这种话不会从我嘴里出来）
```

### Key Principle / 核心原则

**SOUL.md is a living document.** Write V1 on Day 1, then revise every few days. Your agent's personality is tuned through iteration, not one-shot configuration.

**SOUL.md 是活的文件。** 第一天写 V1，用几天后修改。agent 的性格是迭代调出来的，不是一次配好的。

---

## USER.md — Who You Are / 你是谁

Tell the agent about yourself so it doesn't have to ask repeatedly.
告诉 agent 你是谁，这样它不用每次都问。

### Example / 示例

```markdown
# USER.md
- Name: Alex
- Work: 产品设计师 @ 某科技公司，坐标北京
- Timezone: Asia/Shanghai
- Languages: 中文为主，英文工作用
- 习惯：别解释我已经知道的东西
- 当前目标：三月内把副项目上线
- 兴趣：跑步、做饭、机械键盘
```

### Privacy Note / 隐私

Only include what you're comfortable with. This file lives locally, but be thoughtful — especially if you use the agent in group chats.
只写你觉得 OK 的。文件在本地，但如果你在群里用 agent，想清楚哪些信息可以被带出来。

---

## AGENTS.md — How to Work / 怎么协作

The "employee handbook." Defines workflows, rules, and boundaries.
"员工手册"——定义工作流、规则和边界。

### Example: Starter / 入门版

```markdown
# AGENTS.md

## Every Session / 每次启动
1. Read SOUL.md
2. Read USER.md
3. Read memory/ for recent context

## Memory / 记忆
- Write daily notes to memory/YYYY-MM-DD.md
- Important stuff goes to MEMORY.md

## Safety / 安全
- Ask before deleting files
- Ask before sending anything public
- `trash` > `rm`

## Can Do Without Asking / 可以直接做
- Read files, search web
- Organize workspace
- Write memory notes
```

### Example: Advanced / 进阶版

```markdown
# AGENTS.md

## Session Startup
1. Read SOUL.md, USER.md
2. Read NOW.md（current state lifeboat）
3. Read today + yesterday memory files
4. Read self-review.md — check for recurring mistakes

## Memory
- Daily: memory/YYYY-MM-DD.md
- Long-term: MEMORY.md（curated, <200 lines）
- Current state: NOW.md（update frequently）

## Safety
- `trash` > `rm`
- Ask before: rm -rf, sudo, public posts, emails

## Heartbeat
- Check email, calendar 2-4x daily
- Don't disturb 23:00-08:00 unless urgent

## Communication
- Telegram: 用 message tool，长消息拆开发
- 不用 😅
```

---

## The Iteration Loop / 迭代循环

```
Write V1 → Use 2-3 days → Notice what's off → Revise → Repeat
写 V1 → 用几天 → 发现哪里不对 → 改 → 重复
```

Your agent becomes "yours" through this loop. The first version is always wrong. That's fine.
你的 agent 通过这个循环变成"你的"。第一版永远是错的，没关系。第十版会让你觉得它真的懂你。
