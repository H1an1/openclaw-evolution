# Multi-Agent Architecture

One agent is powerful. Multiple agents working together is a system. Here's how to set it up, and when each approach makes sense.

---

## Why Multiple Agents?

Single agent limits:
- **Context window** — One agent can't hold everything (your job + side projects + social media + fitness plan)
- **Personality conflict** — A professional coding assistant and a warm personal companion are hard to be simultaneously
- **Channel isolation** — You might want different agents in different chats
- **Parallel work** — One agent can't code and answer your messages at the same time

---

## Approach A: Single Gateway, Multiple Agents

**Setup:** One OpenClaw instance (one `openclaw.json`), multiple agent configs sharing the same daemon.

### How It Works

```
┌─────────────────────────────────────┐
│           Single Gateway            │
│         (one openclaw.json)         │
│                                     │
│  ┌─────────┐  ┌─────────┐  ┌────┐  │
│  │  Main  │  │  Lily   │  │Kai │  │
│  │(main)   │  │(English)│  │(fit)│  │
│  └────┬────┘  └────┬────┘  └──┬─┘  │
│       │            │          │     │
│  Telegram     Telegram    Telegram  │
│  Bot A        Bot B       Bot C     │
└─────────────────────────────────────┘
```

### Step-by-Step Tutorial

#### 1. Set Up Your First Agent (if you haven't already)

```bash
openclaw onboard
# Follow the wizard — this creates your main agent
```

This gives you a working `~/.openclaw/openclaw.json` with one agent.

#### 2. Create a Telegram Bot for Each Agent

Go to [@BotFather](https://t.me/BotFather) on Telegram:
- `/newbot` → Name it (e.g., "Lily English Tutor") → Get the bot token
- Repeat for each agent you want

You'll end up with multiple bot tokens:
```
Main agent:    7123456789:AAF...  (already configured)
English tutor: 7234567890:BBG...
Fitness coach: 7345678901:CCH...
```

#### 3. Create Separate Workspaces

Each agent needs its own workspace directory:

```bash
mkdir -p ~/lily-workspace
mkdir -p ~/kai-workspace
```

Create identity files for each:

```bash
# Lily's personality
cat > ~/lily-workspace/SOUL.md << 'EOF'
# SOUL.md
I'm Lily, an English tutor. Patient, encouraging, focused on practical conversation skills.
EOF

cat > ~/lily-workspace/USER.md << 'EOF'
# USER.md
[Your info — what Lily needs to know about you as a student]
EOF

cat > ~/lily-workspace/AGENTS.md << 'EOF'
# AGENTS.md
## Rules
- Focus on English learning only
- Write daily notes to memory/
- Be encouraging but correct mistakes
EOF
```

#### 4. Edit openclaw.json — Add Multiple Agents

Open your config:
```bash
nano ~/.openclaw/openclaw.json
```

The key structure:

```json5
{
  // Shared provider config (all agents use these API keys)
  providers: {
    anthropic: { apiKey: "sk-ant-..." }
  },

  // Agent-specific configs
  agents: {
    // Your main agent (already exists from onboard)
    defaults: {
      workspace: "~/main-workspace",
      model: { primary: "anthropic/claude-sonnet-4-5" }
    }
  },

  // Channel configs — each bot token maps to an agent
  channels: {
    telegram: {
      // Main bot (default agent)
      botToken: "7123456789:AAF...",
      dmPolicy: "pairing",
    },

    // Additional Telegram bots as separate accounts
    // Each gets its own agent workspace
  }
}
```

**Important:** OpenClaw supports multiple agents through separate channel accounts. Each Telegram bot token maps to its own agent config. The exact config structure for multi-agent varies by version — run `openclaw onboard` or check `openclaw doctor` for the current schema.

A common pattern:

```json5
{
  agents: {
    defaults: {
      workspace: "~/main-workspace",
      model: { primary: "anthropic/claude-sonnet-4-5" }
    }
  },
  channels: {
    telegram: {
      accounts: {
        // Main agent
        default: {
          botToken: "7123456789:AAF...",
          dmPolicy: "pairing",
          allowFrom: ["tg:YOUR_CHAT_ID"],
        },
        // English tutor
        lily: {
          botToken: "7234567890:BBG...",
          dmPolicy: "pairing",
          allowFrom: ["tg:YOUR_CHAT_ID"],
          agentOverrides: {
            workspace: "~/lily-workspace",
            model: { primary: "anthropic/claude-sonnet-4-5" },
            systemPrompt: "You are Lily, an English tutor."
          }
        },
        // Fitness coach
        kai: {
          botToken: "7345678901:CCH...",
          dmPolicy: "pairing",
          allowFrom: ["tg:YOUR_CHAT_ID"],
          agentOverrides: {
            workspace: "~/kai-workspace",
            model: { primary: "anthropic/claude-sonnet-4-5" },
            systemPrompt: "You are Kai, a fitness coach."
          }
        }
      }
    }
  }
}
```

#### 5. Restart and Verify

```bash
openclaw gateway restart
openclaw status
# Should show all bots connected
```

Send a message to each bot on Telegram. They should respond with their own personality.

#### 6. Verify Workspace Isolation

After chatting with each agent:
```bash
ls ~/main-workspace/memory/   # Main agent's notes
ls ~/lily-workspace/memory/   # Lily's notes
ls ~/kai-workspace/memory/    # Kai's notes
```

Each agent writes to its own directory. No cross-contamination.

### Pros
- **Simple to manage** — One config file, one daemon, one `openclaw gateway restart`
- **Shared resources** — API keys configured once, used by all agents
- **Easy inter-agent communication** — Agents can reference each other via sessions
- **Lower overhead** — One Node.js process

### Cons
- **Single point of failure** — Gateway crashes → all agents down
- **Shared rate limits** — All agents share the same API key quota
- **Shared subscription** — If using Claude Max or similar, all agents eat from the same pool

### Best For
- Personal multi-agent setup (2-5 agents)
- Same person, different-purpose agents
- Getting started with multi-agent

---

## Approach B: Multiple Gateways (Full Isolation)

**Setup:** Completely separate OpenClaw instances, each with its own config, state, and daemon.

### How It Works

```
┌──────────────────┐  ┌──────────────────┐
│   Gateway 1      │  │   Gateway 2      │
│   Port 18789     │  │   Port 19789     │
│                  │  │                  │
│  ┌─────────┐    │  │  ┌─────────┐    │
│  │  Main  │    │  │  │ Worker  │    │
│  │(personal) │    │  │  │ (worker)  │    │
│  └────┬────┘    │  │  └────┬────┘    │
│       │         │  │       │         │
│  Telegram       │  │  Discord        │
│  ~/.openclaw/   │  │  ~/.worker-claw/  │
└──────────────────┘  └──────────────────┘
```

### Step-by-Step Tutorial

#### 1. You Already Have Gateway 1

Your main agent is already running:
```bash
openclaw status
# Gateway running on port 18789
```

Config lives at `~/.openclaw/openclaw.json`.

#### 2. Create Gateway 2 Using Profiles

The cleanest way is OpenClaw's `--profile` flag:

```bash
# Run the onboarding wizard for the second gateway
openclaw --profile worker onboard
```

This creates:
- Config at `~/.openclaw/profiles/worker/openclaw.json` (auto-scoped)
- State dir at `~/.openclaw/profiles/worker/state/` (auto-scoped)
- Separate workspace, credentials, and sessions

During onboarding:
- Set a **different port** (e.g., 19789) — must be at least 20 apart from your main gateway
- Configure its own channel (Telegram bot token, Discord bot token, etc.)
- Set its own workspace directory

#### 3. Configure the Second Gateway

Edit the second gateway's config:

```bash
nano ~/.openclaw/profiles/worker/openclaw.json
```

```json5
{
  gateway: {
    port: 19789  // Different from main gateway!
  },
  providers: {
    anthropic: { apiKey: "sk-ant-..." }  // Can be same or different key
  },
  agents: {
    defaults: {
      workspace: "~/worker-workspace",
      model: { primary: "anthropic/claude-sonnet-4-5" }
    }
  },
  channels: {
    discord: {
      botToken: "DISCORD_BOT_TOKEN",
      allowedGuildIds: ["YOUR_SERVER_ID"],
      allowedChannelIds: ["CHANNEL_1", "CHANNEL_2"]
    }
  }
}
```

#### 4. Start Both Gateways

```bash
# Main gateway (already running, or start it)
openclaw gateway start

# Second gateway
openclaw --profile worker gateway start
```

Both run as separate system services. Each restarts independently.

#### 5. Verify Both Are Running

```bash
openclaw status                    # Main gateway
openclaw --profile worker status     # Second gateway
```

#### 6. Install as System Services

```bash
# Both auto-start on boot
openclaw gateway install                    # Main
openclaw --profile worker gateway install     # Worker
```

### Alternative: Environment Variables (Manual)

If you prefer not to use profiles:

```bash
# Gateway 1 (default)
openclaw gateway start

# Gateway 2 (manual isolation)
OPENCLAW_CONFIG_PATH=~/.worker-openclaw/openclaw.json \
OPENCLAW_STATE_DIR=~/.worker-openclaw/state \
openclaw gateway --port 19789
```

This works but is harder to manage. Profiles are recommended.

### Pros
- **Full isolation** — Each agent is independent. Crash one, others keep running
- **Separate API keys** — Different accounts, different quotas, different billing
- **Different machines** — Run agents on laptop + Raspberry Pi + VPS
- **Security isolation** — Agent A literally cannot access Agent B's files
- **Independent upgrades** — Can run different OpenClaw versions per gateway

### Cons
- **More to manage** — Multiple configs, multiple daemons, multiple logs
- **Inter-agent communication harder** — Agents can't share sessions directly
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
| Setup complexity | Low | Medium |
| Isolation | Shared process | Full isolation |
| Failure blast radius | All agents | One agent |
| API quota | Shared | Separate possible |
| Inter-agent comms | Easy (sessions) | Needs explicit setup |
| Resource usage | Lower | Higher |
| Config management | One file | One per gateway |
| Multi-machine | No | Yes |
| Best for | Personal use | Team / Production |

---

## Practical Tips (From Experience)

1. **Start with single gateway.** Only split when you have a real reason (different machine, different person, isolation needs).

2. **Separate workspaces from Day 1.** Even on single gateway, each agent needs its own directory. Mixing workspaces = agents overwriting each other's memory.

3. **Memory isolation matters.** Agent A should never accidentally read/write Agent B's memory. Set workspace paths carefully and verify with `ls` after a few days.

4. **One "main" agent.** Designate one agent as your primary interface. Others are specialists. If you create 5 equal agents, you'll forget which to talk to.

5. **Manager pattern works.** Main agent reviews specialist agents' daily logs. Creates accountability without micromanagement. Example: main agent reads `~/lily-workspace/memory/YYYY-MM-DD.md` each evening to check on Lily's work.

6. **Port spacing for multi-gateway.** Leave at least 20 ports between gateways. OpenClaw uses derived ports (browser, canvas, CDP) that offset from the base port. Too close = port conflicts.

7. **Same API key is fine for single gateway.** You only need separate keys when you want separate billing or rate limits.
