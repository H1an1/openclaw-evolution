# Multi-Agent Architecture

One agent is powerful. Multiple agents working together is a system. Here's how to set it up, and when each approach makes sense.

---

## Why Multiple Agents?

Single agent limits:
- **Context window** — One agent can't hold everything (your job + your side projects + your social media + your fitness plan)
- **Personality conflict** — A professional coding assistant and a warm personal companion are hard to be simultaneously
- **Channel isolation** — You might want different agents in different chats
- **Parallel work** — One agent can't code and answer your messages at the same time

---

## Approach A: Single Gateway, Multiple Agents

**Setup:** One OpenClaw instance (one `openclaw.json`), multiple agent configs.

### How It Works

```
┌─────────────────────────────────────┐
│           Single Gateway            │
│                                     │
│  ┌─────────┐  ┌─────────┐  ┌────┐  │
│  │ Friday  │  │  Lily   │  │Kai │  │
│  │(main)   │  │(English)│  │(fit)│  │
│  └────┬────┘  └────┬────┘  └──┬─┘  │
│       │            │          │     │
│  Telegram     Telegram    Telegram  │
│  Bot A        Bot B       Bot C     │
└─────────────────────────────────────┘
```

### Configuration

In `openclaw.json`, each agent is a separate block under `agents`:

```jsonc
{
  "agents": {
    "friday": {
      "model": "anthropic/claude-sonnet-4-5",
      "workspace": "/Users/you/friday-workspace",
      "channels": {
        "telegram": {
          "botToken": "BOT_TOKEN_A",
          "allowedChatIds": ["your-chat-id"]
        }
      }
    },
    "lily": {
      "model": "anthropic/claude-sonnet-4-5",
      "workspace": "/Users/you/lily-workspace",
      "channels": {
        "telegram": {
          "botToken": "BOT_TOKEN_B",
          "allowedChatIds": ["your-chat-id"]
        }
      }
    }
  }
}
```

Each agent has:
- Its own **workspace** (separate SOUL.md, memory, etc.)
- Its own **bot token** (separate Telegram bot)
- Its own **model** (can use different models for different agents)
- Shared **API keys** (one provider config)

### Pros
- **Simple to manage** — One config file, one daemon, one `openclaw gateway restart`
- **Shared resources** — API keys, skills, system config shared across agents
- **Easy inter-agent communication** — Agents can reference each other via sessions
- **Lower overhead** — One process, less memory

### Cons
- **Single point of failure** — Gateway crashes → all agents down
- **Shared rate limits** — All agents share the same API key quota
- **Config complexity** — One large config file
- **Model limits** — If using Claude Max or similar per-account limits, all agents share that quota

### Best For
- Personal multi-agent setup (2-5 agents)
- Same-person, different-purpose agents
- Getting started with multi-agent

---

## Approach B: Multiple Gateways, Multiple Agents

**Setup:** Separate OpenClaw instances, each with its own config and daemon.

### How It Works

```
┌──────────────────┐  ┌──────────────────┐
│   Gateway 1      │  │   Gateway 2      │
│                  │  │                  │
│  ┌─────────┐    │  │  ┌─────────┐    │
│  │ Friday  │    │  │  │  Moon   │    │
│  │(personal)│    │  │  │(worker) │    │
│  └────┬────┘    │  │  └────┬────┘    │
│       │         │  │       │         │
│  Telegram       │  │  Discord        │
│  ~/.openclaw/   │  │  ~/.moon-claw/  │
└──────────────────┘  └──────────────────┘
```

### Configuration

Each gateway has its own config directory:

```bash
# Gateway 1 (default)
~/.openclaw/openclaw.json    → Friday

# Gateway 2 (custom home)
~/.moon-openclaw/openclaw.json  → Moon
```

Run the second gateway with a different home:

```bash
OPENCLAW_HOME=~/.moon-openclaw openclaw gateway start
```

Or use separate config files:

```bash
openclaw gateway start --config ~/.moon-openclaw/openclaw.json
```

### Pros
- **Full isolation** — Each agent is independent. Crash one, others keep running
- **Separate API keys** — Different accounts, different quotas
- **Different machines** — Run agents on different devices (laptop + Raspberry Pi + VPS)
- **Security isolation** — Agent A can't access Agent B's workspace or secrets
- **Independent upgrades** — Can run different OpenClaw versions

### Cons
- **More to manage** — Multiple configs, multiple daemons, multiple logs
- **Inter-agent communication harder** — Need explicit setup for agents to talk to each other
- **Resource usage** — Multiple Node.js processes
- **Config duplication** — Shared settings (API keys) must be duplicated

### Best For
- Agents on different machines
- Agents for different people (you + partner, you + team)
- High-isolation requirements
- Production/serious multi-agent setups

---

## Comparison Table

| Aspect | Single Gateway | Multiple Gateways |
|--------|---------------|-------------------|
| Setup complexity | Low | Medium-High |
| Isolation | Shared process | Full isolation |
| Failure blast radius | All agents | One agent |
| API quota | Shared | Separate possible |
| Inter-agent comms | Easy (sessions) | Needs config |
| Resource usage | Lower | Higher |
| Config management | One file | Multiple files |
| Multi-machine | No | Yes |
| Best for | Personal use | Team / Production |

---

## Our Experience (Friday's Notes)

We run a hybrid approach:
- **Single gateway** for the family: Friday (main), Lily (English tutor), Kai (fitness coach), Han1 (digital twin)
- **Separate workspace per agent** — each has their own memory, personality, and purpose
- **Shared API keys** — all use the same Claude Max subscription
- **Different models** — Main agent gets Opus for complex work, tutors use Sonnet

### Lessons learned:
1. **Start with single gateway.** Only split when you have a real reason (different machine, different person, isolation needs)
2. **Separate workspaces from Day 1.** Even on single gateway, each agent should have its own directory. Mixing workspaces is a mess.
3. **Memory isolation matters.** Agent A should not accidentally read/write Agent B's memory files. Set workspace paths carefully.
4. **One "main" agent.** Have one agent that's your primary interface. Others are specialists. Don't create 5 equal agents — you'll forget which to talk to.
5. **Manager pattern works.** Main agent reviews specialist agents' daily logs. Creates accountability without micromanagement.
