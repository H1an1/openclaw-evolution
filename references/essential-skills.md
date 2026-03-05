# Essential Skills — What to Install First

Not all skills are equal. Some are almost universally useful; others are niche. Here's a tiered recommendation.

---

## Tier 1: Install on Day 1

These skills are useful for almost everyone. Install them early.

### weather
- Zero setup, no API key
- Every agent should be able to answer "what's the weather"
- `openclaw skills install weather`

### github
- If you code at all, this is essential
- PR status, issues, CI checks, code review — all from chat
- Needs `gh` CLI installed and authenticated
- `openclaw skills install github`

### summarize
- Summarize URLs, YouTube videos, podcasts, local files
- Great fallback for "what does this article say" or "transcribe this video"
- `openclaw skills install summarize`

---

## Tier 2: Install in Week 1

Based on your workflow, pick what fits.

### For productivity
- **apple-reminders** or **things-mac** — Task management (macOS)
- **gog** — Google Workspace (Gmail, Calendar, Drive, Sheets)
- **himalaya** — Email via IMAP/SMTP (cross-platform)
- **apple-notes** or **bear-notes** or **obsidian** — Note-taking

### For development
- **coding-agent** — Delegate complex coding to Codex/Claude Code/Pi via background process
- **mcporter** — Connect to any MCP server, call tools directly

### For communication
- **imsg** — iMessage/SMS (macOS)
- **wacli** — WhatsApp messages and history
- **discord** — Discord operations beyond basic chat

### For media
- **edge-tts** — Free text-to-speech (Microsoft Edge neural voices, no API key)
- **mlx-whisper** — Local speech-to-text (Apple Silicon optimized)
- **openai-whisper-api** — Cloud speech-to-text (needs OpenAI key)

---

## Tier 3: Install When Needed

Specialized skills — powerful but niche.

- **nano-banana-pro** — AI image generation (Gemini)
- **pollinations** — Multi-model AI generation (images, video, audio)
- **spotify-player** — Spotify playback control
- **openhue** — Philips Hue lights
- **camsnap** — RTSP/ONVIF camera snapshots
- **peekaboo** — macOS UI automation
- **1password** — Secrets management

---

## How to Choose

Ask yourself:
1. **What do I do every day?** → Those tools should be skills
2. **What do I ask Siri/Alexa/Google for?** → Your agent should handle those too
3. **What's annoying and repetitive?** → Automate it with a skill + cron

Don't install everything. Each skill adds to the agent's context window. Install what you use, remove what you don't.

---

## Creating Your Own Skills

When no existing skill fits your workflow:

```bash
openclaw skills create my-skill
```

This generates a skeleton with SKILL.md and a scripts/ directory. Fill in the instructions and your agent learns a new ability.

See the **skill-creator** skill for detailed guidance on structuring custom skills.
