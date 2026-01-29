---
name: delegate
description: Delegate tasks to external AI CLI tools (Codex or Gemini). Smart routing based on task type, or explicit CLI selection with "codex" or "gemini" prefix.
user-invocable: true
---

# Smart Task Delegation

Delegate tasks to external AI CLI tools with intelligent routing.

## Usage

- `/delegate <task>` — automatic routing based on task analysis
- `/delegate codex <task>` — explicitly route to Codex CLI
- `/delegate gemini <task>` — explicitly route to Gemini CLI

## User Task

$ARGUMENTS

## Instructions

### 1. Parse arguments for explicit CLI selector

Check if the first word is an explicit CLI selector:

- If `$ARGUMENTS` starts with `codex ` (case-insensitive):
  - Target: **Codex**
  - Task: everything after "codex "

- If `$ARGUMENTS` starts with `gemini ` (case-insensitive):
  - Target: **Gemini**
  - Task: everything after "gemini "

- Otherwise:
  - Perform auto-routing analysis
  - Task: full `$ARGUMENTS`

### 2. Auto-routing logic (if no explicit selector)

**Route to Codex if:**
- Task involves code: implementation, debugging, refactoring, code review
- Keywords present: fix, debug, refactor, implement, review, test, bug, error, code, patch, build, compile
- Russian keywords: код, исправь, ревью, отладка, баг, ошибка, рефакторинг, тест, напиши, реализуй
- Specific files or paths mentioned (e.g., `.go`, `.ts`, `.py`, `src/`, `cmd/`)
- Syntax errors or runtime issues to fix

**Route to Gemini if:**
- Task is conceptual: explanations, documentation, architecture advice
- Keywords present: explain, what is, how to, document, concept, why, describe, compare, difference, overview
- Russian keywords: объясни, расскажи, документация, концепция, почему, опиши, как работает, сравни, в чём разница
- No specific code changes needed
- General knowledge or architectural questions

### 3. Announce routing decision

Before executing, announce:
- "Routing to **Codex** — [reason]"
- OR "Routing to **Gemini** — [reason]"

### 4. Execute delegation

#### For Codex (via MCP)

Call the `mcp__codex__codex` tool:

```
prompt: |
  Task: {task}

  Requirements:
  - Do NOT apply changes automatically
  - If code changes are needed — provide unified diff (patch)
  - List all affected files
  - Explain reasoning for changes
```

#### For Gemini (via shell)

Execute using Bash tool:

```bash
cat > /tmp/gemini_prompt_$$.txt <<'PROMPT_EOF'
Task: {task}

Requirements:
- Provide clear explanation with examples if helpful
- If code is discussed — show relevant snippets
- Structure response with headers for readability
PROMPT_EOF
gemini -p "$(cat /tmp/gemini_prompt_$$.txt)"
rm -f /tmp/gemini_prompt_$$.txt
```

Note: Using `$$` (process ID) in filename to avoid collisions.

### 5. Synthesize response

After CLI responds:
- Summarize the result (5-10 lines)
- Highlight key conclusions and next steps
- If diff was provided — show it in a fenced `diff` block
- Note any follow-up actions the user might want to take

## Examples

```
/delegate fix the null pointer exception in auth.go
→ Auto-routes to Codex (keyword: fix, file path)

/delegate codex review this function for performance
→ Explicit Codex routing

/delegate gemini explain how JWT tokens work
→ Explicit Gemini routing

/delegate what is the purpose of this middleware?
→ Auto-routes to Gemini (keyword: what is)

/delegate объясни разницу между mutex и channel
→ Auto-routes to Gemini (Russian: объясни)

/delegate исправь баг в функции ParseConfig
→ Auto-routes to Codex (Russian: исправь, баг)
```
