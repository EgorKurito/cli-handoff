---
description: Uninstall cli-handoff plugin and remove global skills
user-invocable: true
---

# Uninstall CLI Handoff

Remove the cli-handoff plugin and its global skill shortcuts.

## Instructions

1) Remove the global /delegate skill directory:

```bash
rm -rf ~/.claude/skills/delegate
```

2) Clean up old skill files (from previous versions, if any):

```bash
rm -f ~/.claude/skills/codex.md ~/.claude/skills/gemini.md ~/.claude/skills/delegate.md
```

3) Confirm removal:

```bash
echo "Removed global skill: delegate"
ls ~/.claude/skills/ 2>/dev/null || echo "Skills directory is empty or does not exist"
```

4) Inform the user:

The cli-handoff plugin has been uninstalled:
- Global shortcut `/delegate` is removed
- Old shortcuts `/codex`, `/gemini` are also cleaned up (if they existed)
- To complete removal, delete the plugin directory or remove it from your plugins list

Note: The plugin commands (`/cli-handoff:*`) will no longer work after removing the plugin directory.
