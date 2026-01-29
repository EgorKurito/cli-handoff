---
description: Check CLI tools and install global skill shortcuts
user-invocable: true
---

Check that Codex CLI and Gemini CLI are installed, and install the global /delegate skill.

## Instructions

1) Check versions (use Bash tool):
   - `codex --version`
   - `gemini --version`

2) If Codex is not installed — suggest:
   ```bash
   npm install -g @openai/codex
   codex login
   ```

3) If Gemini is not installed — suggest:
   ```bash
   npm install -g @google/gemini-cli
   ```

4) Install global /delegate skill:

   Create the skill directory:
   ```bash
   mkdir -p ~/.claude/skills/delegate
   ```

   Read the SKILL.md content from this plugin's `skills/delegate/SKILL.md` using the Read tool, then use the Write tool to save it to `~/.claude/skills/delegate/SKILL.md`.

5) Clean up old skill files (from previous versions):
   ```bash
   rm -f ~/.claude/skills/codex.md ~/.claude/skills/gemini.md ~/.claude/skills/delegate.md
   ```

6) Verify installation:
   ```bash
   ls ~/.claude/skills/delegate/
   ```
   Should show: `SKILL.md`

7) After installation:
   - Remind to restart Claude Code if MCP tools don't appear.
   - Verify that `mcp__codex__codex` tool is available.
   - Confirm: "Global shortcut `/delegate` is now available from any project."
   - Explain usage:
     - `/delegate <task>` — automatic routing
     - `/delegate codex <task>` — explicit Codex
     - `/delegate gemini <task>` — explicit Gemini
