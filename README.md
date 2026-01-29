# cli-handoff

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

## Commands

| Command | Description |
|---------|-------------|
| `/setup` | Check Codex CLI and Gemini CLI installation |
| `/codex <task>` | Delegate task to Codex CLI via MCP |
| `/gemini <task>` | Delegate task to Gemini CLI via shell |

## Requirements

- **Codex CLI**: `npm install -g @openai/codex && codex login`
- **Gemini CLI**: `npm install -g @google/gemini-cli`

## How it works

- **Codex delegation** uses MCP (Model Context Protocol) to call Codex directly
- **Gemini delegation** uses shell commands with `-p` flag for non-interactive queries
- Both commands return synthesized results with diffs when code changes are suggested

## License

MIT
