# Channel Configuration Guide

How to connect your agent to messaging platforms. Each channel has its quirks — here's what you need to know.

---

## Telegram (Recommended for Getting Started)

### Why Start Here
- Simplest setup (one bot token)
- Rich features (buttons, reactions, voice messages, file sharing)
- Fast and reliable
- Works on all devices

### Setup

1. **Create a bot** via [@BotFather](https://t.me/BotFather) on Telegram
   - Send `/newbot`
   - Choose a name and username
   - Copy the bot token

2. **Configure in openclaw.json:**
```jsonc
{
  "channels": {
    "telegram": {
      "botToken": "YOUR_BOT_TOKEN",
      "allowedChatIds": ["YOUR_CHAT_ID"]
    }
  }
}
```

3. **Find your chat ID:**
   - Send a message to the bot
   - Check logs: `openclaw gateway logs` — it will show the chat ID of incoming messages
   - Or use [@userinfobot](https://t.me/userinfobot)

### Essential Settings

```jsonc
{
  "channels": {
    "telegram": {
      "botToken": "YOUR_BOT_TOKEN",
      "allowedChatIds": ["YOUR_CHAT_ID"],
      // Recommended settings:
      "voiceTranscription": true,        // Auto-transcribe voice messages
      "parseMode": "Markdown"             // Rich text formatting
    }
  }
}
```

### Tips
- **allowedChatIds is critical** — Without it, anyone who finds your bot can talk to it
- **Voice messages** — Enable `voiceTranscription` to let your agent understand voice messages (needs a transcription skill like mlx-whisper or groq-whisper)
- **Group chats** — Add the bot to a group, get the group's chat ID (it's negative, like `-1001234567890`), add to allowedChatIds
- **Multiple chats** — One bot can serve multiple chat IDs (private chat + groups)

### Gotchas
- Bot can't initiate conversations — user must message first
- Messages over 4096 characters get split automatically
- Telegram rate limits: ~30 messages/second per bot (rarely hit in personal use)

---

## Discord

### Why Discord
- Multiple channels in one server (organize by topic)
- Thread support (deep conversations without cluttering main channel)
- Multiple bots in one server
- Great for multi-agent setups (each agent = one bot)

### Setup

1. **Create a Discord application** at [Discord Developer Portal](https://discord.com/developers/applications)
2. **Create a bot** under the application
3. **Get the bot token**
4. **Invite the bot** to your server with appropriate permissions

```jsonc
{
  "channels": {
    "discord": {
      "botToken": "YOUR_DISCORD_BOT_TOKEN",
      "allowedGuildIds": ["YOUR_SERVER_ID"],
      "allowedChannelIds": ["CHANNEL_1", "CHANNEL_2"]
    }
  }
}
```

### Essential Settings

```jsonc
{
  "channels": {
    "discord": {
      "botToken": "YOUR_DISCORD_BOT_TOKEN",
      "allowedGuildIds": ["YOUR_SERVER_ID"],
      "allowedChannelIds": ["CHANNEL_ID_1", "CHANNEL_ID_2"],
      // Behavior settings:
      "requireMention": true,             // Only respond when @mentioned (recommended for shared servers)
      "allowBots": false                  // Don't respond to other bots (prevent loops!)
    }
  }
}
```

### Tips
- **requireMention** — In shared servers, set to `true` so the bot only responds when @mentioned. In private servers, `false` is fine.
- **allowBots** — Set to `true` ONLY if you want agents to talk to each other. Use with caution — can create infinite loops. Add behavior rules to prevent this.
- **Channel isolation** — Use `allowedChannelIds` to restrict which channels the bot listens to. Don't let it see everything.
- **Thread-bound sessions** — Great for focused work. Agent spawns a thread, works inside it, reports back.

### Multi-Agent Discord Setup

If you have multiple agents in one server:

```
Server: my-workspace
├── #general        → Main agent responds
├── #coding         → Coding agent responds  
├── #fitness        → Fitness agent responds
└── #agent-chat     → Both agents, allowBots=true (controlled chaos)
```

Each agent's config only lists its own channels in `allowedChannelIds`.

### Gotchas
- Discord has a 2000-character message limit (shorter than Telegram!)
- No native markdown tables — use bullet lists instead
- Embeds suppress with `<url>` wrapping
- Rate limits are stricter than Telegram

---

## Other Channels

OpenClaw supports many channels. Brief notes on each:

### WhatsApp (wacli)
- Requires the wacli skill and WhatsApp Web pairing
- Good for reaching people who only use WhatsApp
- No markdown tables, limited formatting

### Signal
- Privacy-focused
- Requires Signal CLI setup
- Good for sensitive communications

### iMessage (imsg)
- macOS only
- Requires the imsg skill
- Great if your contacts are Apple users

### Slack
- Good for work environments
- Similar to Discord in concept

---

## General Channel Tips

1. **Start with ONE channel.** Get it working perfectly before adding more.
2. **Different channels, different purposes.** Telegram for quick personal chat, Discord for organized project work.
3. **Test with allowedChatIds/allowedGuildIds.** Always restrict access. An open bot is a security risk.
4. **Voice transcription** is a killer feature. Set it up if your channel supports voice messages.
5. **Format for the channel.** What works in Telegram (markdown tables) doesn't work in Discord or WhatsApp. Train your agent to adapt.
