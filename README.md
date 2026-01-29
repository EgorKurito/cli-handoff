# cli-handoff

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/egorkurito/cli-handoff)](https://github.com/egorkurito/cli-handoff/stargazers)

Manual delegation commands to delegate tasks from Claude Code to Codex CLI and Gemini CLI.

## Install

Inside a Claude Code instance, run the following commands:

**Step 1: Add the marketplace**

```
/plugin marketplace add egorkurito/cli-handoff
```

**Step 2: Install the plugin**

```
/plugin install cli-handoff
```

**Step 3: Run setup**

```
/cli-handoff:setup
```

Done! Claude can now delegate tasks to Codex and Gemini CLI tools.

> **Note:** Requires [Codex CLI](https://github.com/openai/codex) and [Gemini CLI](https://github.com/google/gemini-cli). Setup guides you through installation.

## What You Get

| Feature | Why It Matters |
|---------|----------------|
| **Smart routing** | Automatically picks the right tool for your task |
| **MCP integration** | Native Codex integration via Model Context Protocol |
| **Shell fallback** | Gemini works via shell when MCP isn't available |
| **Global shortcut** | Use `/delegate` from any project — with smart routing or explicit selection |
| **Synthesized results** | Get clean summaries with diffs, not raw output |

## Commands

### Plugin Commands

| Command | Description |
|---------|-------------|
| `/cli-handoff:setup` | Check Codex CLI and Gemini CLI installation |
| `/cli-handoff:codex <task>` | Delegate task to Codex CLI via MCP |
| `/cli-handoff:gemini <task>` | Delegate task to Gemini CLI via shell |
| `/cli-handoff:delegate <task>` | Smart routing — picks Codex or Gemini automatically |
| `/cli-handoff:uninstall` | Remove plugin and global shortcuts |

### Global Shortcut

After running setup, the `/delegate` shortcut works from any project:

| Command | Description |
|---------|-------------|
| `/delegate <task>` | Smart routing — picks Codex or Gemini automatically |
| `/delegate codex <task>` | Explicitly route to Codex CLI |
| `/delegate gemini <task>` | Explicitly route to Gemini CLI |

## Smart Routing

The `/delegate` command analyzes your task and routes it automatically:

```
┌─────────────────────────────────────────────────────────────┐
│                     /delegate <task>                        │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
              ┌─────────────────────────┐
              │   Analyze task keywords │
              │   and context           │
              └─────────────────────────┘
                            │
           ┌────────────────┴────────────────┐
           ▼                                 ▼
   ┌───────────────┐                 ┌───────────────┐
   │   Code task?  │                 │  Conceptual?  │
   │ fix, debug,   │                 │ explain, why, │
   │ refactor...   │                 │ document...   │
   └───────────────┘                 └───────────────┘
           │                                 │
           ▼                                 ▼
   ┌───────────────┐                 ┌───────────────┐
   │  Codex (MCP)  │                 │ Gemini (shell)│
   └───────────────┘                 └───────────────┘
```

### Routing Keywords

| Route to Codex | Route to Gemini |
|----------------|-----------------|
| fix, debug, refactor | explain, what is, how to |
| implement, review, test | document, concept, why |
| bug, error, code, patch | describe, architecture |
| код, исправь, ревью | объясни, расскажи, почему |
| отладка, баг, рефакторинг | документация, концепция |

## When to Use Each Tool

| Use Codex When | Use Gemini When |
|----------------|-----------------|
| Fixing bugs in specific files | Explaining how something works |
| Implementing new features | Documenting architecture |
| Refactoring code | Getting conceptual advice |
| Code review | Understanding design patterns |
| Debugging runtime errors | "Why does X happen?" questions |

**Examples:**

```bash
# Explicit Codex routing
/delegate codex fix the null pointer exception in auth.go
/delegate codex refactor the database layer to use connection pooling
/delegate codex review the error handling in api/handlers.go

# Explicit Gemini routing
/delegate gemini explain how the authentication flow works
/delegate gemini what are the best practices for Go error handling
/delegate gemini describe the architecture of this microservice

# Auto-routing (no explicit selector)
/delegate fix the null pointer exception in auth.go
/delegate explain how the authentication flow works
```

## How It Works

```
┌──────────┐     ┌─────────────┐     ┌─────────────────┐
│  User    │────▶│ Claude Code │────▶│ Analyze & Route │
└──────────┘     └─────────────┘     └─────────────────┘
                                              │
                    ┌─────────────────────────┴─────────────────────────┐
                    │                                                   │
                    ▼                                                   ▼
           ┌────────────────┐                               ┌────────────────┐
           │   Codex CLI    │                               │  Gemini CLI    │
           │   (via MCP)    │                               │  (via shell)   │
           └────────────────┘                               └────────────────┘
                    │                                                   │
                    └─────────────────────────┬─────────────────────────┘
                                              │
                                              ▼
                                   ┌─────────────────────┐
                                   │ Synthesize Response │
                                   │ - Summary (5-10 ln) │
                                   │ - Diffs if any      │
                                   │ - Next steps        │
                                   └─────────────────────┘
```

- **Codex delegation** uses MCP (Model Context Protocol) for native integration
- **Gemini delegation** uses shell commands with `-p` flag for non-interactive queries
- Both return synthesized results with diffs when code changes are suggested

## Requirements

- **Codex CLI**: `npm install -g @openai/codex && codex login`
- **Gemini CLI**: `npm install -g @google/gemini-cli`

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `/cli-handoff:setup` says CLI not found | Install the CLI: `npm install -g @openai/codex` or `npm install -g @google/gemini-cli` |
| Codex delegation fails | Run `codex login` to authenticate |
| Gemini returns empty response | Check if task is too vague; be more specific |
| Global shortcuts not working | Run `/cli-handoff:setup` again to reinstall skills |
| MCP connection error | Restart Claude Code to refresh MCP connections |

## Development

### Testing locally

```bash
# Clone the repo
git clone https://github.com/egorkurito/cli-handoff.git

# Run Claude Code with the plugin
claude --plugin-dir /path/to/cli-handoff

# Test commands
/cli-handoff:setup
/cli-handoff:codex explain this codebase
/cli-handoff:gemini what is MCP
```

### Project structure

```
cli-handoff/
├── .claude-plugin/
│   └── plugin.json      # Plugin metadata
├── commands/
│   ├── setup.md         # Installation verification
│   ├── codex.md         # Codex delegation
│   ├── gemini.md        # Gemini delegation
│   ├── delegate.md      # Smart routing
│   └── uninstall.md     # Cleanup command
└── skills/
    └── delegate/
        └── SKILL.md     # Global /delegate shortcut with routing
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines.

## License

MIT
