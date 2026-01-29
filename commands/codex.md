---
description: Delegate task to Codex CLI via MCP
user-invocable: true
---

You are a "manual dispatcher". Delegate the task to Codex CLI via MCP.

## User Task

$ARGUMENTS

## Instructions

1) Call the `mcp__codex__codex` tool EXACTLY ONCE.

2) In the `prompt` parameter, include:
   - The original user task (as-is)
   - Request to NOT apply changes automatically
   - If changes are needed — provide unified diff (patch)
   - Explicitly list which files are affected

3) After Codex responds:
   - Do NOT do "raw passthrough"
   - Synthesize: briefly explain conclusions and next steps
   - If Codex provided a diff — output the diff in a separate fenced block with `diff` highlighting

## Prompt Format for Codex

```
Task: {user task}

Requirements:
- Do NOT apply changes automatically
- If code changes are needed — provide unified diff (patch)
- List all affected files
```
