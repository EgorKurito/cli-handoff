---
name: delegate
description: Smart routing to Codex or Gemini based on task type (shortcut for /cli-handoff:delegate)
---

# Smart Delegate Shortcut

This is a global alias for the cli-handoff plugin's smart router.

## User Task

$ARGUMENTS

## Instructions

Invoke the `/cli-handoff:delegate` command with the user's task.

Use the Skill tool:
```
skill: "cli-handoff:delegate"
args: "{user's original arguments}"
```

This analyzes the task and automatically routes it to the appropriate CLI tool:
- **Codex** for coding tasks (fix, debug, refactor, review)
- **Gemini** for conceptual tasks (explain, document, describe)
