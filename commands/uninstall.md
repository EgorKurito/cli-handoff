---
description: Uninstall cli-handoff plugin and remove global skills
user-invocable: true
---

# Uninstall CLI Handoff

Remove the cli-handoff plugin and its global skill shortcuts.

## Instructions

1) Remove global skill shortcuts from `~/.claude/skills/`:

```bash
rm -f ~/.claude/skills/codex.md ~/.claude/skills/gemini.md ~/.claude/skills/delegate.md
```

2) Confirm removal:

```bash
echo "Removed global skills: codex, gemini, delegate"
ls ~/.claude/skills/ 2>/dev/null || echo "Skills directory is empty or does not exist"
```

3) Inform the user:

The cli-handoff plugin has been uninstalled:
- Global shortcuts `/codex`, `/gemini`, `/delegate` are removed
- To complete removal, delete the plugin directory or remove it from your plugins list

Note: The plugin commands (`/cli-handoff:*`) will no longer work after removing the plugin directory.
