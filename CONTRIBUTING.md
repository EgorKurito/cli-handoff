# Contributing to cli-handoff

Thank you for your interest in contributing! This guide will help you get started.

## Quick Start

```bash
# Clone the repository
git clone https://github.com/egorkurito/cli-handoff.git
cd cli-handoff

# Run Claude Code with the plugin loaded
claude --plugin-dir .

# Test the commands
/cli-handoff:setup
```

## What to Contribute

| Area | Examples |
|------|----------|
| **New CLI tools** | Add support for other AI CLIs (Aider, Continue, etc.) |
| **Routing rules** | Improve `/delegate` keyword matching |
| **Bug fixes** | Fix command issues, edge cases |
| **Documentation** | Examples, translations, clarifications |
| **Skills** | New global shortcuts or aliases |

## Project Structure

```
cli-handoff/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata (name, version)
├── commands/
│   ├── setup.md             # Installation verification
│   ├── codex.md             # Codex delegation (MCP)
│   ├── gemini.md            # Gemini delegation (shell)
│   ├── delegate.md          # Smart routing
│   └── uninstall.md         # Cleanup
├── skills/
│   ├── codex.md             # Global /codex shortcut
│   ├── gemini.md            # Global /gemini shortcut
│   └── delegate.md          # Global /delegate shortcut
├── docs/
│   └── plans/               # Planning documents (not in git)
├── README.md                # User documentation
├── CLAUDE.md                # Developer/Claude instructions
└── CONTRIBUTING.md          # This file
```

## Adding a New CLI Tool

1. Create `commands/<tool>.md` with the delegation logic
2. Create `skills/<tool>.md` for global shortcut
3. Update `commands/setup.md` to verify installation
4. Update `commands/delegate.md` routing keywords
5. Update `commands/uninstall.md` cleanup
6. Update README.md with new command

### Command File Template

```markdown
---
description: Delegate task to <Tool> CLI
user-invocable: true
---

# <Tool> Delegation

## User Task

$ARGUMENTS

## Instructions

1. Build the prompt
2. Execute delegation (MCP or shell)
3. Synthesize response
```

## Pull Request Process

1. **Test locally** — run commands with `claude --plugin-dir .`
2. **Update docs** — if adding features, update README.md
3. **Atomic commits** — one logical change per commit
4. **Clear description** — explain what and why

## Commit Message Format

```
<type>: <description>

Types:
- feat: New feature
- fix: Bug fix
- docs: Documentation only
- refactor: Code change without feature/fix
- chore: Maintenance (version bump, etc.)
```

**Examples:**
```
feat: add Aider CLI delegation support
fix: handle empty responses from Gemini
docs: add troubleshooting section to README
chore: bump version to 0.3.0
```

## Testing

Manual testing is the primary method:

```bash
# 1. Load the plugin
claude --plugin-dir /path/to/cli-handoff

# 2. Verify setup
/cli-handoff:setup
# Expected: Shows CLI versions for installed tools

# 3. Test each command
/cli-handoff:codex explain this file
# Expected: Codex response synthesized

/cli-handoff:gemini what is MCP
# Expected: Gemini response synthesized

/cli-handoff:delegate fix the bug in main.go
# Expected: Routes to Codex, shows reasoning

/cli-handoff:delegate explain the architecture
# Expected: Routes to Gemini, shows reasoning

# 4. Test global shortcuts
/codex test task
/gemini test task
/delegate test task
```

## Code Style

- **Markdown** — use fenced code blocks with language hints
- **YAML frontmatter** — required for command files
- **Comments** — explain non-obvious logic
- **ASCII art** — OK for diagrams, keep simple

## Questions?

Open an issue on [GitHub](https://github.com/egorkurito/cli-handoff/issues).
