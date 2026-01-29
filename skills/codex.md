---
name: codex
description: Delegate coding task to Codex CLI (shortcut for /cli-handoff:codex)
---

# Codex CLI Shortcut

This is a global alias for the cli-handoff plugin's Codex command.

## User Task

$ARGUMENTS

## Instructions

Invoke the `/cli-handoff:codex` command with the user's task.

Use the Skill tool:
```
skill: "cli-handoff:codex"
args: "{user's original arguments}"
```

This delegates coding tasks (debugging, refactoring, code review, implementation) to Codex CLI via MCP.
