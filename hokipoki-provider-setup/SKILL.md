---
name: hokipoki-provider-setup
description: Guide for becoming a HokiPoki provider and sharing your AI subscriptions to earn credits
---

# HokiPoki Provider Setup Skill

## What is a Provider?

Providers share their AI subscriptions (Claude Max, Codex, Gemini) with others via HokiPoki, making AI tools accessible to the community.

## Why Become a Provider?

1. **Help the community** - Share access to AI tools
2. **Team collaboration** - Let teammates use your subscription
3. **Maximize usage** - Get more value from your AI subscriptions

## Prerequisites

Before becoming a provider, ensure you have:

1. **HokiPoki CLI installed:**
   ```bash
   npm install -g @next-halo/hokipoki-cli
   ```

2. **HokiPoki account:**
   ```bash
   hokipoki login
   ```

3. **AI subscriptions** - At least one of:
   - Claude (Claude Max recommended)
   - Codex (OpenAI subscription)
   - Gemini (Google AI subscription)

## Registration Steps

### Step 1: Register as Provider

```bash
hokipoki register --as-provider --tools claude codex gemini
```

This command:
- Opens browser for each tool to authenticate
- Stores encrypted OAuth tokens locally
- Registers you as a provider in the network

### Step 2: Verify Registration

```bash
hokipoki status --provider
```

Expected output:
```
✅ Provider Status: Active
Available Tools: claude, codex, gemini
Tasks Completed: 0
```

### Step 3: Start Listening

```bash
hokipoki listen --tools claude codex gemini
```

Options:
- `--tools <tools>` - Specify which AIs to share (required)
- `--port <port>` - P2P connection port (default: 9090)
- `--server <url>` - Relay server URL (default: wss://relay.hoki-poki.ai)
- `--secure` - Run in production mode with Docker sandboxing

## Security Best Practices

### Secure Mode (Recommended for Production)

Always use `--secure` flag when running as a provider:

```bash
hokipoki listen --tools claude --secure
```

This enables:
- LUKS-encrypted Docker containers
- Read-only filesystem
- Memory-only workspace (tmpfs)
- Automatic cleanup after each task
- No code retention

### Token Security

- OAuth tokens are encrypted at rest
- Tokens are never exposed to requesters
- Tokens are injected into containers securely
- Refresh tokens are auto-rotated

## Task Execution Flow

When a provider accepts a task:

1. **Task Received** - Relay server routes matching task
2. **Code Clone** - Clone from requester's ephemeral git server
3. **Container Setup** - Create encrypted Docker container
4. **AI Execution** - Run claude/codex/gemini with OAuth token
5. **Patch Generation** - Capture code changes as git patch
6. **Results Submission** - Push patch back to requester
7. **Cleanup** - Wipe container and encryption key

## Managing Provider Settings

### List Available Tools

```bash
hokipoki status
```

### Add New Tool

```bash
hokipoki register --as-provider --tools gemini
```

### Remove Tool

```bash
# Edit ~/.hokipoki/config.json and remove tool
# Then restart listener
```

### Update OAuth Tokens

```bash
# For Claude
claude setup-token

# For Codex
codex login

# For Gemini
gemini
```

Then re-register:
```bash
hokipoki register --as-provider --tools claude
```

## Monitoring and Analytics

### View Provider Stats

```bash
hokipoki status --provider
```

Shows:
- Tasks completed
- Tool usage breakdown
- Average execution time

### Open Dashboard

```bash
hokipoki dashboard
```

Web dashboard shows:
- Real-time task queue
- Task completion history
- Tool performance metrics
- Provider statistics

## Troubleshooting

### Container Fails to Start

```bash
# Check Docker is running
docker ps

# Verify Docker permissions
docker run hello-world

# Check disk space
df -h
```

### OAuth Token Expired

```bash
# Re-authenticate with AI tool
claude setup-token  # or codex login, or gemini

# Re-register
hokipoki register --as-provider --tools claude
```

### Port Already in Use

```bash
# Use different port
hokipoki listen --tools claude --port 9091

# Or find and kill existing process
lsof -i :9090
kill <PID>
```

## Best Practices

1. **Run in background** - Use process manager like pm2:
   ```bash
   pm2 start "hokipoki listen --tools claude --secure" --name hokipoki-provider
   ```

2. **Monitor logs** - Check for errors regularly:
   ```bash
   pm2 logs hokipoki-provider
   ```

3. **Set resource limits** - Limit CPU/memory for Docker containers:
   ```bash
   # Edit Docker daemon.json
   {
     "default-ulimits": {
       "cpu": { "soft": 2, "hard": 4 },
       "memlock": { "soft": -1, "hard": -1 }
     }
   }
   ```

4. **Whitelist workspaces** - Only serve specific teams:
   ```bash
   hokipoki listen --tools claude --workspace my-team
   ```


## Community Guidelines

As a provider, please:

1. **Maintain uptime** - Aim for >95% availability
2. **Accept all tasks fairly** - Help all community members equally
3. **Keep tokens fresh** - Update OAuth tokens when they expire
4. **Report issues** - Help improve the platform

## Support

Need help? Contact us:
- **Documentation**: [hoki-poki.ai/docs/provider](https://hoki-poki.ai/docs/provider)
- **Discord**: [discord.gg/hokipoki](https://discord.gg/hokipoki)
- **Email**: provider-support@hoki-poki.ai
