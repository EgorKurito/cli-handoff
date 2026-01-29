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
/cli-handoff:setup      # Verify CLI installations, install global skill
/cli-handoff:codex      # Test Codex delegation
/cli-handoff:gemini     # Test Gemini delegation
/cli-handoff:delegate   # Test smart routing
/cli-handoff:uninstall  # Remove global skill
```

## Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                         User Request                             │
│              "/delegate fix the bug" or                          │
│              "/delegate codex review this function"              │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                        Claude Code                               │
│         Loads skill from skills/delegate/SKILL.md                │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                      Argument Parsing                            │
│   "codex <task>" → explicit Codex                                │
│   "gemini <task>" → explicit Gemini                              │
│   "<task>" → auto-route by keywords                              │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                      Routing Analysis                            │
│         (if auto-routing: examine keywords)                      │
│         "fix" → code task → Codex                                │
│         "explain" → conceptual → Gemini                          │
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

1. **User invokes command** — `/delegate [codex|gemini] <task>`
2. **Parse arguments** — check for explicit CLI selector prefix
3. **Route decision**:
   - If explicit selector → use that CLI
   - Otherwise → auto-route based on keywords
4. **Execute delegation**:
   - Codex: Call `mcp__codex__codex` MCP tool
   - Gemini: Execute `gemini -p "prompt"` via shell
5. **Synthesize response** — summarize output, format diffs

## Usage Examples

```
/delegate fix the null pointer exception in auth.go
→ Auto-routes to Codex (keyword: fix, file path)

/delegate codex review this function for performance
→ Explicit Codex routing

/delegate gemini explain how JWT tokens work
→ Explicit Gemini routing

/delegate what is the purpose of this middleware?
→ Auto-routes to Gemini (keyword: what is)
```

## Smart Routing Logic

The `/delegate` command uses keyword matching for auto-routing:

| Criteria | Routes To |
|----------|-----------|
| Code modification keywords: fix, debug, refactor, implement, review, test, bug, error, code, patch, build, compile | Codex |
| Specific file paths mentioned (.go, .ts, .py, src/, cmd/) | Codex |
| Conceptual keywords: explain, what is, how to, document, concept, why, describe, compare, difference | Gemini |
| No code changes needed | Gemini |

**Russian keywords are also supported:**
- Codex: код, исправь, ревью, отладка, баг, ошибка, рефакторинг, тест, напиши, реализуй
- Gemini: объясни, расскажи, документация, концепция, почему, опиши, как работает, сравни

## Component Relationships

| Component | Purpose | Location |
|-----------|---------|----------|
| `plugin.json` | Plugin metadata (name, version, author) | `.claude-plugin/` |
| Command files | Slash commands (`/cli-handoff:*`) | `commands/` |
| Skill directory | Global `/delegate` shortcut | `skills/delegate/` → installed to `~/.claude/skills/delegate/` |

### Command vs Skill

- **Commands** are plugin-scoped: `/cli-handoff:codex`, `/cli-handoff:gemini`, `/cli-handoff:delegate`
- **Skill** is a global shortcut: `/delegate` (works from any project)
- Skill is installed to `~/.claude/skills/delegate/` during `/cli-handoff:setup`

### Unified Skill Design

The plugin uses a single unified `/delegate` skill instead of separate `/codex` and `/gemini` shortcuts:
- Simpler mental model — one command to remember
- Explicit selection still available via prefix: `/delegate codex <task>`
- Auto-routing handles most cases intelligently

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
- Process ID in filename to avoid collisions

### 3. Synthesize, Don't Passthrough

Raw CLI output is often verbose. The plugin:
- Summarizes responses to 5-10 lines
- Formats diffs in proper code blocks
- Highlights actionable next steps

### 4. Dual-Language Keywords

Routing supports both English and Russian keywords to serve broader user base.

### 5. Directory-based Skill Format

Skills use the proper directory format (`skills/delegate/SKILL.md`) following Claude Code conventions, enabling:
- Future expansion with supporting files
- Consistent structure with other skills
- Better organization

## File Purposes

| File | Purpose |
|------|---------|
| `commands/setup.md` | Verifies CLI installation, installs global `/delegate` skill |
| `commands/codex.md` | Codex delegation via MCP with structured prompt |
| `commands/gemini.md` | Gemini delegation via shell with prompt file |
| `commands/delegate.md` | Smart router with keyword analysis |
| `commands/uninstall.md` | Removes global skill, cleanup instructions |
| `skills/delegate/SKILL.md` | Global `/delegate` skill with routing and explicit selection |
