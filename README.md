# Hermes Agent - Complete Guide

<a href="https://github.com/nousresearch/hermes-agent"><img src="https://img.shields.io/badge/GitHub-NousResearch/hermes_agent-blue?logo=github" alt="GitHub"></a>
<a href="https://pypi.org/project/hermes-agent/"><img src="https://img.shields.io/pypi/v/hermes-agent?logo=pypi" alt="PyPI"></a>
<a href="https://pypi.org/publish"><img src="https://img.shields.io/pypi/pyversions/hermes-agent" alt="Python"></a>
<a href="https://github.com/nousresearch/hermes-agent/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-MIT-green" alt="License"></a>

Hermes Agent is an open-source AI agent that runs on your server with persistent memory, multi-platform messaging integration (Telegram, Discord, Slack, WhatsApp), scheduled automations, and extensible tool-calling capabilities.

## Features

- **Persistent Memory**: Learns and improves across sessions
- **Multi-platform**: Works on CLI, Telegram, Discord, Slack, WhatsApp
- **Automation**: Schedule recurring tasks with cron
- **Extensible**: MCP servers, skills, plugins
- **Self-learning**: Creates and refines skills over time

## Requirements

- Python 3.10+
- API key for your LLM provider (OpenAI, Anthropic, Ollama, etc.)

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

This script will:
- Download and set up Hermes Agent
- Create necessary directories (~/.hermes)
- Set up configuration files
- Add hermes command to PATH

## Quick Start

### Initial Setup

```bash
hermes setup
```

The setup wizard will guide you through:
1. Selecting your LLM provider (OpenAI, Anthropic, Ollama, etc.)
2. Entering API keys
3. Choosing a default model
4. Setting up messaging platforms (optional)

### Interactive Chat

Start an interactive chat session:

```bash
hermes
```

### Single Query

Run a one-shot query without interactive mode:

```bash
hermes chat -q "What is the capital of France?"
```

### Health Check

Verify your installation is working correctly:

```bash
hermes doctor
```

## Core Workflow

### Interactive Chat

The primary way to interact with Hermes - a continuous conversation:

```bash
hermes
```

This starts an interactive session where:
- You type messages and Hermes responds
- Conversation history is preserved
- Hermes can use tools and skills
- Memory persists across turns

### Session Management

| Command | Description |
|---------|------------|
| `/new` or `/reset` | Start a fresh session |
| `/clear` | Clear screen and new session (CLI) |
| `/resume [name]` | Resume a previous session |
| `/history` | View conversation history |
| `/save` | Save conversation to disk |
| `/title [name]` | Name current session |
| `/exit` or `Ctrl+D` | End session |

## Slash Commands

Type `/` in chat to see available commands.

### Session Commands

| Command | Usage | Description |
|---------|-------|-------------|
| `/new` | `/new` | Start new session with fresh ID |
| `/reset` | `/reset` | Alias for /new |
| `/clear` | `/clear` | Clear screen, new session (CLI) |
| `/history` | `/history` | Display conversation history |
| `/save` | `/save` | Save conversation permanently |
| `/retry` | `/retry` | Resend last message |
| `/undo` | `/undo` | Remove last exchange |
| `/title` | `/title My Session` | Set session name |
| `/resume` | `/resume [name]` | Resume named session |
| `/branch` | `/branch [name]` | Branch session for exploration |
| `/stop` | `/stop` | Kill background processes |

### Context & Token Commands

| Command | Usage | Description |
|---------|-------|-------------|
| `/compress` | `/compress [focus]` | Summarize context, preserve focus |
| `/compact` | `/compact [focus]` | Compress context (CLAUDE.md survives) |
| `/context` | `/context` | Visualize context usage |
| `/usage` | `/usage` | Check token consumption |
| `/cost` | `/cost` | Token usage breakdown |

### Tool Commands

| Command | Usage | Description |
|---------|-------|-------------|
| `/help` | `/help` | Show all commands |
| `/tools` | `/tools` | List available tools |
| `/toolsets` | `/toolsets` | List tool collections |
| `/skills` | `/skills` | Browse/search skills |
| `/skill` | `/skill <name>` | Load specific skill |
| `/plugins` | `/plugins` | List plugins |
| `/cron` | `/cron` | Manage cron jobs |
| `/reload-mcp` | `/reload-mcp` | Reload MCP servers |

### Messaging & Convenience Commands

| Command | Usage | Description |
|---------|-------|-------------|
| `/model` | `/model` | Switch model/provider |
| `/personality` | `/personality pirate` | Change personality |
| `/btw` | `/btw <question>` | Side question (no context cost) |
| `/background` | `/background <prompt>` | Run in background session |
| `/queue` | `/queue <prompt>` | Queue for next turn |
| `/status` | `/status` | Session info |
| `/todos` | `/todos` | List action items |
| `/debug` | `/debug` | Upload debug report |

## Tools & Extensions

### MCP Servers

MCP (Model Context Protocol) servers add external tools to Hermes. Configure in `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
```

### Skills System

Skills extend Hermes with specialized capabilities.

Browse available skills:

```bash
hermes skills browse
```

Search for skills:

```bash
hermes skills search kubernetes --source skills-sh
```

Install skills:

```bash
hermes skills install openai/skills/k8s
hermes skills install official/security/1password
```

## Automation

### Cron Jobs

Schedule recurring tasks to run automatically:

Using `/cron` in chat:

```bash
/cron add 30m "Remind me to check the build"
/cron add "every 2h" "Check server status"
/cron add "every 1h" "Summarize new feed items" --skill blogwatcher
```

Using CLI:

```bash
hermes cron create "0 9 * * 1" "Generate a weekly AI news digest" --name "Weekly digest" --deliver telegram
hermes cron create "every 1h" "Use both skills" --skill blogwatcher --skill find-nayyy --name "Skill combo"
```

List Cron Jobs:

```bash
hermes cron list
```

### Background Tasks

Run tasks without blocking:

```bash
/background research the latest news on autonomous agents
```

Results delivered back when complete. Your session stays free for other work.

### Queue System

Queue prompts for next turn:

```bash
/queue Write a summary of our conversation so far
```

## Messaging Platforms

### Telegram

1. Message @BotFather on Telegram
2. Create a new bot with /newbot
3. Get the bot token
4. Add to `~/.hermes/.env`:

```env
TELEGRAM_ENABLED=true
TELEGRAM_BOT_TOKEN=your_bot_token
```

### Discord

1. Go to Discord Developer Portal
2. Create an application and bot
3. Get the bot token
4. Add to `~/.hermes/.env`:

```env
DISCORD_ENABLED=true
DISCORD_BOT_TOKEN=your_bot_token
```

### Slack

1. Create a Slack app at api.slack.com
2. Enable Bot Token Scope
3. Install to workspace
4. Add to `~/.hermes/.env`:

```env
SLACK_ENABLED=true
SLACK_BOT_TOKEN=xoxb-your-token
SLACK_SIGNING_SECRET=your_secret
```

### WhatsApp

```env
WHATSAPP_ENABLED=true
WHATSAPP_MODE=bot  # or "self-chat"
WHATSAPP_ALLOWED_USERS=15551234567,15559876543
```

### Gateway

The gateway manages all messaging platforms:

```bash
hermes gateway setup
hermes gateway start
hermes gateway stop
hermes gateway status
hermes gateway install  # System service
```

## Memory System

### Session Memory

Hermes automatically remembers conversation context within a session:
- Messages are preserved in context
- Use `/compress` to summarize and save tokens
- Use `/undo` to remove last exchange

### Long-term Memory

Use Honcho for persistent memory across sessions:

```bash
hermes memory setup      # Interactive configuration
hermes memory status     # Check active provider
hermes memory off        # Disable external provider
```

#### Honcho Tools

| Tool | Usage | Description |
|------|-------|-------------|
| `honcho_profile` | `honcho_profile` | Fast warmup, no LLM cost |
| `honcho_context` | `honcho_context` | Full snapshot (no LLM) |
| `honcho_conclude` | `honcho_conclude conclusion="..."` | Save to memory |
| `honcho_search` | `honcho_search query="..."` | Fast search (no LLM) |
| `honcho_reasoning` | `honcho_reasoning query="..."` | Synthesized answer (LLM) |

### User Profile

Enable user profile persistence in config:

```yaml
memory:
  memory_enabled: true
  user_profile_enabled: true
```

## Configuration

### config.yaml

Main configuration file at `~/.hermes/config.yaml`:

```yaml
# Model configuration
model:
  provider: "openai"  # openai, anthropic, ollama, etc.
  model: "gpt-4"
   
# Display settings
display:
  compact: false
  personality: "default"  # default, pirate, kawaii, etc.
   
# Memory settings  
memory:
  memory_enabled: true
  user_profile_enabled: true
   
# Context compression
compression:
  enabled: true
  threshold: 0.50
  target_ratio: 0.20
  protect_last_n: 20
```

### Environment Variables

Store sensitive values in `~/.hermes/.env`:

```env
# API Keys
OPENAI_API_KEY=sk-xxx
ANTHROPIC_API_KEY=sk-ant-xxx

# Messaging Platforms
TELEGRAM_BOT_TOKEN=xxx
DISCORD_BOT_TOKEN=xxx
SLACK_BOT_TOKEN=xoxb-xxx

# MCP Servers (only safe vars passed)
GITHUB_PERSONAL_ACCESS_TOKEN=ghp_xxx
```

### Model/Provider Setup

```bash
hermes model
hermes config edit
```

## Power User Tips

### Context Optimization

Keep sessions efficient:

- Use `/compress focus on the current task` before limits
- Check token consumption: `/usage`
- View context visualization: `/context`
- See cost breakdown: `/cost`

### Token Saving Strategies

| Strategy | How |
|----------|-----|
| Use /btw | Quick questions without history |
| Use /compress | Summarize and reduce context |
| Focus parameter | `/compress focus on auth` |
| Start fresh | `/new` for new topics |
| Compact mode | `/compact` keeps CLAUDE.md |

### Efficient Workflows

1. **Start with honcho_profile** - Fast warmup, no LLM cost
2. **Use honcho_search** - For specific facts, no LLM
3. **Use honcho_reasoning** - Only when deep synthesis needed
4. **Don't re-fetch injected context** - It auto-injects!

### Branching Sessions

```bash
/branch experiment-with-llama-3
```

Explore different paths while preserving the original.

### Snapshots

```bash
/snapshot create before-refactor
/snapshot restore 1
/snapshot prune 5
```

## Troubleshooting

### Debug Reports

```bash
/hermes
/debug
```

CLI alternative:

```bash
hermes debug share              # Upload, print URL
hermes debug share --lines 500  # More logs
hermes debug share --local     # Print locally
```

### Health Checks

```bash
hermes doctor
```

### Common Issues

- **API key not working**: Check ~/.hermes/.env
- **Slow responses**: Use /compress
- **Platform not connecting**: Check tokens in .env
- **Tools not appearing**: Check MCP config and reload with /reload-mcp

## Quick Reference

### CLI Commands

```bash
hermes                 # Interactive chat
hermes chat -q "query"  # Single query
hermes setup            # Setup wizard
hermes model            # Change model
hermes doctor           # Health check
hermes config edit      # Edit config
hermes tools list     # List tools
hermes skills browse   # Browse skills
hermes skills install <skill>
hermes cron create "<schedule>" "<prompt>"
hermes cron list
hermes gateway setup
hermes gateway start/stop/status
hermes memory setup/status/off
hermes sessions browse
hermes debug share
```

---

<p style="text-align: center; color: #6e7681;">Hermes Agent Complete Guide | Built with ❤️</p>
<p style="text-align: center; font-size: 14px; color: #6e7681;">Official Docs: <a href="https://github.com/nousresearch/hermes-agent">github.com/nousresearch/hermes-agent</a></p>