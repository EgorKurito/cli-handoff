---
name: gemini
description: Delegate task to Gemini CLI (shortcut for /cli-handoff:gemini)
---

# Gemini CLI Shortcut

This is a global alias for the cli-handoff plugin's Gemini command.

## User Task

$ARGUMENTS

## Instructions

Invoke the `/cli-handoff:gemini` command with the user's task.

Use the Skill tool:
```
skill: "cli-handoff:gemini"
args: "{user's original arguments}"
```

This delegates conceptual tasks (explanations, documentation, architecture advice) to Gemini CLI via shell.
