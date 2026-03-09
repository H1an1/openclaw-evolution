# Troubleshooting / 常见问题排查

Something not working? Check here first.
出了问题？先看这里。

---

## Agent 不回消息

**症状：** 发了消息但没有回复。

**排查步骤：**
1. `openclaw status` — 检查 gateway 是否在运行
2. `openclaw gateway logs --tail 20` — 看最近的日志有没有报错
3. 检查频道配置 — bot token 是否正确，是否连了正确的频道
4. 在 Telegram 里给 bot 发 `/start`，有些 bot 需要先激活

**常见原因：**
- Gateway 没启动：`openclaw gateway start`
- API key 过期或额度用完：检查你的 model provider 账户
- Bot token 失效：去 @BotFather 重新获取

---

## Cron 没触发

**症状：** 设了 cron 但到时间没有动作。

**排查步骤：**
1. `openclaw cron list` — 确认 cron 存在且 status 不是 `disabled`
2. 检查时区 — `--tz Asia/Shanghai` 是否写对
3. `openclaw cron runs --id <id> --limit 3` — 看历史执行记录，是否有 error
4. Gateway 是否在运行 — cron 依赖 gateway 调度

**常见原因：**
- 时区写错了（默认是 UTC）
- Gateway 在 cron 触发时没在运行
- cron 表达式写错：`0 8 * * *` 是每天 8 点，不是 `8 * * * *`（那是每小时第 8 分钟）

---

## Cron 发出来的消息带思考过程

**症状：** cron announce 的消息开头有一段英文 reasoning 或内部思考。

**原因：** isolated + announce 模式下，agent 的所有输出（包括 thinking/narration）都会被当成 summary 发出来。

**修复：** 在 cron 的 message 前面加：
```
【重要：只输出最终要发给用户的消息，不要输出任何思考过程、分析步骤或元数据。】
```

Or use `openclaw cron edit <id> --message "新的 prompt"`

---

## Heartbeat 太频繁 / 太安静

**太频繁：**
- 调整 interval：在 `openclaw.json` 的 `agents.defaults.heartbeat.every` 改成 `30m` 或 `1h`
- 在 HEARTBEAT.md 里加判断逻辑：不是每次都要做事

**太安静：**
- 确认 heartbeat 配置了：`agents.defaults.heartbeat.every` 不为空
- 检查 HEARTBEAT.md 是否存在
- Agent 可能每次都返回 HEARTBEAT_OK — 在 HEARTBEAT.md 里写清楚什么时候该主动说话

---

## web_search / web_fetch 不能用

**web_search 报错 "no API key"：**
- 需要 Brave Search API key
- 在 `openclaw.json` 里设置：`tools.web.search.apiKey`
- 免费 tier: https://brave.com/search/api/

**web_fetch 报错 "blocked: private IP"：**
- 某些网站 DNS 解析到内网 IP，被安全策略拦截
- 这是正常的安全限制，无法绕过
- 替代方案：用 `web_search` 获取摘要，或用 `browser` 工具访问

---

## Agent 记忆丢失 / 每次都像新的

**原因：** 没有设置 memory 系统。

**修复：**
1. 在 AGENTS.md 里加记忆规则（见 three-files-guide.md）
2. 确保 agent 在写 `memory/YYYY-MM-DD.md`
3. 确保 session 启动时读取了 memory 文件
4. 对于 cron/isolated session，memory 不会自动带入 — 需要在 prompt 里明确让 agent 读文件

---

## 多个 Agent 共享一台机器时互相干扰

**症状：** Agent A 的 memory 被 Agent B 覆盖。

**原因：** 不同 agent 写到了同一个 memory 目录。

**修复：** 每个 agent 用独立的 workspace 和 config：
```bash
# Agent A
OPENCLAW_HOME=~/.agent-a-openclaw openclaw gateway start

# Agent B  
OPENCLAW_HOME=~/.agent-b-openclaw openclaw gateway start
```

或在同一 workspace 里用不同的 memory 文件夹：
```markdown
# In AGENTS.md
- Main chat: write to memory/
- Discord server: write to group-memory/server-name/
```

---

## 还是搞不定？

1. Check OpenClaw docs: https://docs.openclaw.ai
2. Ask in Discord community: https://discord.com/invite/clawd
3. Search GitHub issues: https://github.com/openclaw/openclaw/issues
4. Browse skills on ClawHub: https://clawhub.com
