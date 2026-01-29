---
description: Check CLI tools and install global skill shortcuts
user-invocable: true
---

Check that Codex CLI and Gemini CLI are installed, and provide installation commands if needed.

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

4) Install global skill shortcuts:
   ```bash
   mkdir -p ~/.claude/skills
   ```
   Then copy skill files from this plugin's `skills/` directory to `~/.claude/skills/`:
   - `codex.md`
   - `gemini.md`
   - `delegate.md`

   Use the Read tool to get each file's content from the plugin directory, then Write tool to save to `~/.claude/skills/`.

5) Verify installation:
   ```bash
   ls ~/.claude/skills/
   ```
   Should show: `codex.md`, `delegate.md`, `gemini.md`

6) After installation:
   - Remind to restart Claude Code if MCP tools don't appear.
   - Verify that `mcp__codex__codex` tool is available.
   - Confirm: "Global shortcuts /codex, /gemini, /delegate are now available from any project."
