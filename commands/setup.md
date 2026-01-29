---
description: Проверка установки Codex CLI и Gemini CLI
user-invocable: true
---

Проверь, что установлены Codex CLI и Gemini CLI, и подскажи команды установки.

## Инструкции

1) Проверь версии (используй Bash tool):
   - `codex --version`
   - `gemini --version`

2) Если Codex не установлен — предложи:
   ```bash
   npm install -g @openai/codex
   codex login
   ```

3) Если Gemini не установлен — предложи:
   ```bash
   npm install -g @google/gemini-cli
   ```

4) После установки:
   - Напомни перезапустить Claude Code, если MCP-инструменты не появились.
   - Проверь, что инструмент `mcp__codex__codex` доступен.
