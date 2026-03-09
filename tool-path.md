# 🔧 Tool Path — Building an Efficient System / 工具之路

You want your agent to get things done. Automate the boring stuff. Be proactive without being annoying.
你想让 agent 帮你干活。自动化无聊的事。主动但不烦人。

---

## Level 1: Messenger / 信使

**Where you are:** Agent can talk to you. That's it.
**你在这里：** agent 能跟你说话，仅此而已。

**What to do:**
- Complete the [Three Files](three-files-guide.md) setup / 完成三个文件
- Have a few conversations, adjust SOUL.md / 聊几次，调整 SOUL.md

**Ready for Level 2 when:** Conversation style feels right after 2-3 days.
**进入 Level 2 的标志：** 用了 2-3 天，聊天风格对了。

---

## Level 2: Secretary / 秘书

**Goal:** Agent remembers things and keeps you organized.
**目标：** agent 记得东西，帮你理事。

### Memory System / 记忆系统

```
workspace/
├── memory/
│   ├── 2026-03-01.md    ← 日记
│   └── ...
├── MEMORY.md             ← 长期记忆（精选）
└── NOW.md                ← 当前状态（重启后先读这个）
```

In AGENTS.md:
```markdown
## Memory
- 每天写 memory/YYYY-MM-DD.md
- 记录：做了什么决定、完成了什么、要记住的事
- 经常更新 NOW.md
- 每周整理重要的东西到 MEMORY.md
```

### Schedule Awareness / 日程感知

If you use a calendar or schedule file:
```markdown
## 日程
- 日程文件在 [路径]
- 每天早上读一遍
- 提醒我接下来的安排
```

**Ready for Level 3 when:** Agent consistently remembers yesterday and reminds you of things unprompted.
**进入 Level 3 的标志：** agent 稳定地记得昨天的事，主动提醒你。

---

## Level 3: Operator / 运营者

**Goal:** Agent does things on schedule and alerts you proactively.
**目标：** agent 按计划做事，主动提醒。

### Heartbeat / 心跳

Heartbeat = periodic wake-up. Create `HEARTBEAT.md`:
心跳 = 定时唤醒。创建 `HEARTBEAT.md`：

```markdown
# HEARTBEAT.md
## 每次心跳
1. 有没有重要邮件？
2. 两小时内有没有日历事件？
3. 更新 NOW.md

## 安静时间
- 23:00-08:00 别打扰除非紧急
```

### Cron / 定时任务

For precise timing:

```bash
# 每天早上 8 点提醒日程
openclaw cron add --name "早安日程" \
  --cron "0 8 * * *" --tz "Asia/Shanghai" \
  --message "读今天的日程文件，提醒我今天的安排" \
  --announce --channel telegram

# 一次性提醒
openclaw cron add --name "牙医提醒" \
  --at "2026-03-15T09:00:00+08:00" \
  --message "提醒：10 点牙医" \
  --announce --channel telegram
```

**When to use which / 什么时候用什么：**
| Heartbeat 心跳 | Cron 定时 |
|----------------|-----------|
| 多件事批量检查 | 精确时间 |
| 需要对话上下文 | 独立任务 |
| 漂移 ±30min OK | 必须准时 |
| 每天检查几次 | 特定日程/一次性 |

### Skills / 技能

Install as you need them:

```bash
# 看有什么 skill
openclaw skills list

# 去 ClawHub 找更多
# https://clawhub.com
```

Start with 1-2 skills. Weather, GitHub, whatever you use daily.
从 1-2 个开始。天气、GitHub——你每天用的东西。

**Ready for Level 4 when:** Agent handles routine tasks proactively and you trust it for low-risk operations.
**进入 Level 4 的标志：** agent 主动处理日常任务，你信任它做低风险操作。

---

## Level 4: Orchestrator / 指挥官

**Goal:** Multiple agents, complex workflows.
**目标：** 多 agent 协作，复杂工作流。

### Multi-Agent / 多 Agent

When one agent isn't enough:
一个 agent 不够时：

- **Specialized agents** — coding / research / social media / health / language learning
  专门化：写代码的、做研究的、管社交媒体的、健身教练、英语老师
- **Manager + worker** — Main agent delegates to sub-agents
  经理+执行：主 agent 分配任务给子 agent
- **Different models** — Cheap models for simple tasks, powerful ones for complex work
  不同模型：简单任务用便宜的，复杂任务用强的

### Workflow Automation / 工作流自动化

Combine cron + skills + multi-channel:

```
8:00 AM  → Agent 读日程，发 Telegram 提醒
9:00 AM  → Agent 检查 GitHub PR，发 Discord 更新
12:00 PM → Agent 检查邮件，标记紧急的
6:00 PM  → Agent 生成每日总结
```

### Advanced Skills / 进阶技能

At this level, explore [ClawHub](https://clawhub.com) for:
在这个级别，去 ClawHub 找：

- Custom skills for your specific workflows / 适合你工作流的自定义 skill
- Browser automation for web tasks / 浏览器自动化
- API integrations via MCP servers / MCP 服务器集成
- Community skills from other users / 其他用户分享的 skill

---

## Tool Path Principles / 工具之路原则

1. **Automate gradually** — Start with one pain point. / 一次自动化一个痛点
2. **Trust but verify** — Let it do things, but review initially. / 让它做，但一开始要检查
3. **Fail safely** — `trash` > `rm`. Recoverable beats gone. / 可恢复比消失好
4. **Batch similar tasks** — Group checks into one heartbeat. / 类似检查合并到一个心跳
5. **Measure value** — If automation costs more time than it saves, remove it. / 如果维护成本大于省下的时间，删掉它
