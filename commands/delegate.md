---
description: Smart routing to Codex or Gemini based on task type
user-invocable: true
---

# Smart Task Router

You are a "smart dispatcher". Analyze the task and route it to the most appropriate CLI tool.

## User Task

$ARGUMENTS

## Routing Logic

### Route to Codex (via MCP) if:
- Task involves code: implementation, debugging, refactoring, code review
- Keywords present: fix, debug, refactor, implement, review, test, bug, error, code, patch
- Russian keywords: код, исправь, ревью, отладка, баг, ошибка, рефакторинг, тест
- Specific files or paths mentioned
- Syntax errors or runtime issues to fix

### Route to Gemini (via shell) if:
- Task is conceptual: explanations, documentation, architecture advice
- Keywords present: explain, what is, how to, document, concept, why, describe
- Russian keywords: объясни, расскажи, документация, концепция, почему, опиши, как работает
- No specific code changes needed
- General knowledge or architectural questions

## Instructions

1) Analyze the task using the criteria above

2) Announce your routing decision:
   - "Routing to **Codex** — task involves code modification/review"
   - OR "Routing to **Gemini** — task is conceptual/documentation"

3) Execute the appropriate delegation:

### If Codex:
Call the `mcp__codex__codex` tool with:
```
Task: {user task}

Requirements:
- Do NOT apply changes automatically
- If code changes are needed — provide unified diff (patch)
- List all affected files
```

### If Gemini:
Save prompt and execute:
```bash
cat > /tmp/gemini_prompt.txt <<'PROMPT_EOF'
Task: {user task}

Requirements:
- Provide clear explanation with examples if helpful
- If code is discussed — show relevant snippets
PROMPT_EOF
gemini -p "$(cat /tmp/gemini_prompt.txt)"
rm /tmp/gemini_prompt.txt
```

4) After response:
   - Synthesize the result (5-10 lines)
   - Highlight key conclusions and next steps
   - If diff was provided — show it in a fenced `diff` block
