# CLAUDE.md

Instructions for Claude Code and developers working on this plugin.

## What This Is

A Claude Code plugin for delegating tasks to external AI CLI tools:
- **Codex CLI** — via MCP (Model Context Protocol)
- **Gemini CLI** — via shell commands

## Development Commands

```bash
# Run Claude Code with this plugin loaded
claude --plugin-dir /path/to/cli-handoff

# Test the plugin
/cli-handoff:setup      # Verify CLI installations
/cli-handoff:codex      # Test Codex delegation
/cli-handoff:gemini     # Test Gemini delegation
/cli-handoff:delegate   # Test smart routing
/cli-handoff:uninstall  # Remove global shortcuts
```

## Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                         User Request                             │
│                    "/delegate fix the bug"                       │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                        Claude Code                               │
│              Loads command from commands/delegate.md             │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                      Routing Analysis                            │
│         Examines keywords: "fix" → code task → Codex             │
└──────────────────────────────────────────────────────────────────┘
                                │
           ┌────────────────────┴────────────────────┐
           │                                         │
           ▼                                         ▼
┌─────────────────────────┐             ┌─────────────────────────┐
│       Codex CLI         │             │      Gemini CLI         │
│   mcp__codex__codex     │             │   gemini -p "prompt"    │
│   (native MCP tool)     │             │   (shell execution)     │
└─────────────────────────┘             └─────────────────────────┘
           │                                         │
           └────────────────────┬────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                    Response Synthesis                            │
│    - 5-10 line summary                                           │
│    - Unified diff if code changes                                │
│    - Next steps and recommendations                              │
└──────────────────────────────────────────────────────────────────┘
```

## How Delegation Works

1. **User invokes command** — `/delegate`, `/codex`, or `/gemini`
2. **Claude loads command file** — from `commands/*.md`
3. **Claude builds prompt** — combines user task with instructions
4. **Execute delegation**:
   - Codex: Call `mcp__codex__codex` MCP tool
   - Gemini: Execute `gemini -p "prompt"` via shell
5. **Synthesize response** — summarize output, format diffs

## Smart Routing Logic

The `/delegate` command uses keyword matching to route tasks:

| Criteria | Routes To |
|----------|-----------|
| Code modification keywords: fix, debug, refactor, implement, review, test, bug, error, code, patch | Codex |
| Specific file paths mentioned | Codex |
| Conceptual keywords: explain, what is, how to, document, concept, why, describe | Gemini |
| No code changes needed | Gemini |

**Russian keywords are also supported:**
- Codex: код, исправь, ревью, отладка, баг, ошибка, рефакторинг, тест
- Gemini: объясни, расскажи, документация, концепция, почему, опиши

## Component Relationships

| Component | Purpose | Location |
|-----------|---------|----------|
| `plugin.json` | Plugin metadata (name, version, author) | `.claude-plugin/` |
| Command files | Slash commands (`/cli-handoff:*`) | `commands/` |
| Skill files | Global shortcuts (`/codex`, `/gemini`, `/delegate`) | `skills/` → installed to `~/.claude/skills/` |

### Command vs Skill

- **Commands** are plugin-scoped: `/cli-handoff:codex`
- **Skills** are global shortcuts: `/codex` (works from any project)
- Skills are installed to `~/.claude/skills/` during `/cli-handoff:setup`

## Key Design Decisions

### 1. MCP for Codex

Codex CLI has native MCP server support. Using MCP provides:
- Direct tool invocation without shell overhead
- Better error handling
- Structured responses

### 2. Shell for Gemini

Gemini CLI lacks MCP support, so we use shell execution:
- `-p` flag for non-interactive prompt mode
- Temporary file for multi-line prompts
- Works with standard shell redirection

### 3. Synthesize, Don't Passthrough

Raw CLI output is often verbose. The plugin:
- Summarizes responses to 5-10 lines
- Formats diffs in proper code blocks
- Highlights actionable next steps

### 4. Dual-Language Keywords

Routing supports both English and Russian keywords to serve broader user base.

## File Purposes

| File | Purpose |
|------|---------|
| `commands/setup.md` | Verifies CLI installation, installs global shortcuts |
| `commands/codex.md` | Codex delegation via MCP with structured prompt |
| `commands/gemini.md` | Gemini delegation via shell with prompt file |
| `commands/delegate.md` | Smart router with keyword analysis |
| `commands/uninstall.md` | Removes global shortcuts, cleanup instructions |
| `skills/codex.md` | Global `/codex` alias |
| `skills/gemini.md` | Global `/gemini` alias |
| `skills/delegate.md` | Global `/delegate` alias |
