---
description: Check Codex CLI and Gemini CLI installation
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

4) After installation:
   - Remind to restart Claude Code if MCP tools don't appear.
   - Verify that `mcp__codex__codex` tool is available.
