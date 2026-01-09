# HokiPoki Skills for Claude Code

Official skills for HokiPoki P2P AI marketplace. These skills help Claude Code make intelligent decisions about when and how to use HokiPoki for AI context switching, provider management, and team collaboration.

## Available Skills

### 1. hokipoki-context-switching

**When to Use:** Helps Claude Code decide when to switch to a different AI via HokiPoki.

**Key Features:**
- Decision tree for context switching
- Best practices for multi-file tasks
- Tool selection guidance (Claude vs. Codex vs. Gemini)
- Usage examples and patterns

**Install:**
```bash
/plugin install hokipoki-context-switching@hokipoki-skills
```

### 2. hokipoki-provider-setup

**When to Use:** Guide for becoming a HokiPoki provider and sharing AI subscriptions.

**Key Features:**
- Step-by-step provider registration
- Security best practices
- Task execution flow
- Credit earning strategies
- Troubleshooting common issues

**Install:**
```bash
/plugin install hokipoki-provider-setup@hokipoki-skills
```

### 3. hokipoki-team-collaboration

**When to Use:** Setting up and using HokiPoki team workspaces.

**Key Features:**
- Workspace creation and management
- Task routing strategies
- Credit management
- Team analytics
- Advanced workspace features

**Install:**
```bash
/plugin install hokipoki-team-collaboration@hokipoki-skills
```

## Installation

### Install All Skills

```bash
# Add skills repository
/plugin marketplace add next-halo/hokipoki-skills

# Install individual skills
/plugin install hokipoki-context-switching@hokipoki-skills
/plugin install hokipoki-provider-setup@hokipoki-skills
/plugin install hokipoki-team-collaboration@hokipoki-skills
```

Or install the full plugin which bundles all skills:
```bash
/plugin marketplace add next-halo/hokipoki-plugin
/plugin install hokipoki
```

## Usage

Once installed, skills are automatically available to Claude Code. Claude will reference these skills when:

1. **Context Switching** - You ask "Should I use a different AI for this?"
2. **Provider Setup** - You want to become a provider
3. **Team Collaboration** - You're setting up team workspaces

### Example Conversations

**With hokipoki-context-switching:**
```
You: I've been debugging this auth bug for 30 minutes with no progress

Claude: Based on the hokipoki-context-switching skill, this is a good time
to use /hokipoki to switch to a different AI for a fresh perspective:

/hokipoki --tool codex Fix authentication flow in auth/login.ts

A different AI model might approach the problem differently.
```

**With hokipoki-provider-setup:**
```
You: How do I become a HokiPoki provider?

Claude: Based on the hokipoki-provider-setup skill, here's how:

1. Install HokiPoki CLI: npm install -g @next-halo/hokipoki-cli
2. Register as provider: hokipoki register --as-provider --tools claude
3. Start listening: hokipoki listen --tools claude --secure

The --secure flag is important for production use as it enables encrypted
Docker containers and automatic cleanup.
```

**With hokipoki-team-collaboration:**
```
You: Can my team share one Claude subscription?

Claude: Yes! Based on the hokipoki-team-collaboration skill, you can set up
a team workspace:

1. Create workspace: hokipoki workspace create --name "Engineering Team"
2. Invite team: Share the invite link
3. Start provider: hokipoki listen --tools claude --workspace "Engineering Team"

Then team members can use:
/hokipoki --workspace "Engineering Team" <task>
```

## Skill Structure

Each skill follows this format:

```markdown
---
name: skill-name
description: Brief description
---

# Skill Title

## Content organized in sections
- When to use
- How to use
- Best practices
- Examples
- Troubleshooting
```

## Contributing

Want to add your own HokiPoki skill?

1. Fork this repository
2. Create new skill directory: `your-skill-name/`
3. Add `SKILL.md` with frontmatter and content
4. Submit pull request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## Skill Development Tips

### Progressive Disclosure

Keep SKILL.md concise. Skills share Claude's context window, so:

- Put essential information in SKILL.md
- Link to detailed docs for reference
- Use examples over lengthy explanations
- Break complex topics into multiple skills

### Good Skill Example

```markdown
---
name: hokipoki-quick-start
description: Quick start guide for HokiPoki
---

# Quick Start

## Install
npm install -g @next-halo/hokipoki-cli

## Authenticate
hokipoki login

## Use
/hokipoki Fix the bug in app.ts

See full docs: hoki-poki.ai/docs
```

### Bad Skill Example (Too Verbose)

```markdown
---
name: hokipoki-everything
description: Everything about HokiPoki
---

# Complete HokiPoki Guide

[50 pages of documentation...]
```

## Links

- **Main Plugin:** [hokipoki-plugin](https://github.com/next-halo/hokipoki-plugin)
- **CLI Tool:** [hokipoki-cli](https://github.com/next-halo/hokipoki-cli)
- **Website:** [hoki-poki.ai](https://hoki-poki.ai)
- **Documentation:** [hoki-poki.ai/docs](https://hoki-poki.ai/docs)

## Support

- **Issues:** [github.com/next-halo/hokipoki-skills/issues](https://github.com/next-halo/hokipoki-skills/issues)
- **Discord:** [discord.gg/hokipoki](https://discord.gg/hokipoki)
- **Email:** skills@hoki-poki.ai

## License

MIT License - see [LICENSE](LICENSE) for details.

---

**Made with ❤️ by the HokiPoki Team**

Teach Claude to never get stuck.
