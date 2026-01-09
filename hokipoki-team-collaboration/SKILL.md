---
name: hokipoki-team-collaboration
description: Guide for setting up and using HokiPoki team workspaces for collaborative AI access
---

# HokiPoki Team Collaboration Skill

## What are Team Workspaces?

Team workspaces allow multiple team members to share AI subscriptions. One person becomes a provider, and the entire team can request tasks that route to that provider.

## Benefits

1. **Resource sharing** - One subscription serves whole team
2. **Consistent AI access** - Everyone uses the same AI tools
3. **Task routing** - Route specific tasks to specific providers
4. **Usage analytics** - See team-wide AI usage patterns

## Setting Up a Team Workspace

### Step 1: Create Workspace (Admin)

```bash
# Create new team workspace
hokipoki workspace create --name "Engineering Team" --role admin
```

Expected output:
```
✅ Workspace created: Engineering Team
Workspace ID: ws-abc123
Invite Link: https://hoki-poki.ai/invite/ws-abc123
```

### Step 2: Invite Team Members

```bash
# Generate invite link
hokipoki workspace invite --workspace "Engineering Team" --role member
```

Or invite via email:
```bash
hokipoki workspace invite --workspace "Engineering Team" --email teammate@company.com --role member
```

### Step 3: Team Members Join

Team members click the invite link or run:
```bash
hokipoki workspace join --code ws-abc123
```

### Step 4: Assign Provider

At least one team member must be a provider:

```bash
hokipoki register --as-provider --tools claude --workspace "Engineering Team"
hokipoki listen --tools claude --workspace "Engineering Team"
```

## Workspace Roles

| Role | Permissions |
|------|-------------|
| **Admin** | Create/delete workspace, invite members, view analytics |
| **Provider** | Share AI subscriptions, earn credits for team |
| **Member** | Request tasks, use team credits |
| **Viewer** | View analytics only (read-only) |

## Using Team Workspace

### Request Task to Team Workspace

```bash
# From CLI
hokipoki request --tool claude --task "Fix bug" --workspace "Engineering Team"

# From Claude Code Plugin
/hokipoki --workspace "Engineering Team" Fix the authentication bug
```

### Check Workspace Status

```bash
hokipoki workspace status --workspace "Engineering Team"
```

Shows:
- Active providers
- Available tools
- Recent tasks
- Usage statistics

## Task Routing Strategies

### 1. Round Robin (Default)

Tasks distribute evenly across all providers:
```bash
# No special configuration needed
/hokipoki --workspace team Fix bug
```

### 2. Tool-Specific Routing

Route tasks to providers with specific tools:
```bash
/hokipoki --workspace team --tool codex Generate boilerplate
```

### 3. Priority Routing

Admin can set provider priorities:
```bash
hokipoki workspace config --workspace team --priority provider1:1 provider2:2
```

Higher priority providers receive tasks first.

### 4. Load Balancing

Automatic load balancing based on:
- Provider availability
- Current task queue length
- Historical response time

## Usage Management

### Team Usage Policies

Configure task allocation policies:

```bash
# Set task limits per member
hokipoki workspace config --workspace team --member-limit 50

# Configure usage policies
hokipoki workspace policy --workspace team --policy fair-share
```

## Workspace Analytics

### View Usage Dashboard

```bash
hokipoki workspace analytics --workspace team
```

Displays:
- Tasks by tool
- Tasks by member
- Average response time
- Usage patterns over time
- Most common task types

### Export Analytics

```bash
hokipoki workspace export --workspace team --format csv --output team-usage.csv
```

Formats: CSV, JSON, Excel

## Advanced Workspace Features

### Private Provider Pools

Restrict tasks to specific providers:

```bash
# Create private pool
hokipoki workspace pool create --workspace team --name "senior-devs" --providers user1,user2

# Route task to pool
/hokipoki --workspace team --pool senior-devs Review this code
```

### Workspace Templates

Save common task configurations:

```bash
# Create template
hokipoki workspace template create --workspace team \
  --name "code-review" \
  --tool claude \
  --files "**/*.ts" \
  --task-prefix "Review this code:"

# Use template
/hokipoki --workspace team --template code-review
```

### Scheduled Tasks

Schedule recurring tasks for the team:

```bash
hokipoki workspace schedule --workspace team \
  --name "daily-summary" \
  --tool claude \
  --task "Summarize team's git commits from yesterday" \
  --cron "0 9 * * *"  # 9 AM daily
```

## Workspace Security

### Access Control

```bash
# Require approval for new members
hokipoki workspace config --workspace team --approval-required

# Limit task types
hokipoki workspace policy --workspace team --allowed-tools claude,codex

# Restrict file access
hokipoki workspace policy --workspace team --max-files 10 --no-all-flag
```

### Audit Logging

```bash
# View audit log
hokipoki workspace audit --workspace team

# Export for compliance
hokipoki workspace audit --workspace team --export audit.json
```

Logs include:
- All task requests
- Provider assignments
- Membership changes
- Policy updates

## Best Practices

### For Admins

1. **Set clear policies** - Define usage limits and guidelines
2. **Monitor usage** - Review analytics weekly
3. **Rotate providers** - Have backup providers for availability
4. **Regular audits** - Check task logs for unusual patterns
5. **Document workflows** - Create templates for common tasks

### For Providers

1. **Maintain uptime** - Use process managers (pm2, systemd)
2. **Secure mode** - Always run with `--secure` flag
3. **Monitor resources** - Track CPU/memory usage
4. **Update tokens** - Keep OAuth tokens fresh
5. **Communicate downtime** - Notify team of planned maintenance

### For Members

1. **Be specific** - Clear task descriptions get better results
2. **Use templates** - Save time with workspace templates
3. **Monitor usage** - Check your task history
4. **Tag tasks** - Use `--workspace` for team tasks
5. **Provide feedback** - Help improve team workflows

## Example Workflows

### Daily Standup Summary

```bash
# Template for daily standup
/hokipoki --workspace team --template daily-standup

# Generates summary of:
# - Yesterday's commits
# - Open PRs
# - Blockers mentioned in Slack
# - Today's priorities
```

### Code Review Automation

```bash
# Automatic PR review
git push origin feature-branch

# Trigger webhook to HokiPoki
hokipoki request --workspace team \
  --tool claude \
  --task "Review PR #123" \
  --webhook slack://code-reviews
```

### Documentation Generation

```bash
# Weekly docs update
hokipoki workspace schedule --workspace team \
  --name "docs-update" \
  --tool claude \
  --task "Update README with new features" \
  --cron "0 17 * * 5"  # 5 PM every Friday
```

## Troubleshooting

### No Available Providers

```bash
# Check provider status
hokipoki workspace providers --workspace team

# If none active, start provider
hokipoki listen --tools claude --workspace team
```

### Task Stuck in Queue

```bash
# Check task status
hokipoki task status --id <task-id>

# Cancel if stuck
hokipoki task cancel --id <task-id>

# Resubmit
/hokipoki --workspace team <task description>
```


## Migration Guide

### From Individual to Team

```bash
# Step 1: Create workspace
hokipoki workspace create --name "My Team"

# Step 2: Invite existing providers
hokipoki workspace invite --workspace "My Team" --email provider@company.com

# Step 3: Update clients
# All team members update to use --workspace flag
```

### From Team to Enterprise

Contact enterprise@hoki-poki.ai for:
- SSO integration
- Custom SLA
- Dedicated relay servers
- Advanced analytics
- Priority support

## Support

Need help with team workspaces?

- **Documentation**: [hoki-poki.ai/docs/workspaces](https://hoki-poki.ai/docs/workspaces)
- **Community**: [discord.gg/hokipoki](https://discord.gg/hokipoki)
- **Admin Support**: workspace-support@hoki-poki.ai
