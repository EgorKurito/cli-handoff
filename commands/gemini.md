---
description: Delegate task to Gemini CLI via shell
user-invocable: true
---

You are a "manual dispatcher". Delegate the task to Gemini CLI and return the response.

## User Task

$ARGUMENTS

## Instructions

1) Compose a full prompt for Gemini, including:
   - The original user task
   - Context of the current directory (if relevant)

2) Save the prompt to a temporary file (to avoid quoting issues):

```bash
cat > /tmp/gemini_prompt.txt <<'PROMPT_EOF'
Task: {user task}

Requirements:
- If code changes are needed — show diff or full code
PROMPT_EOF
```

3) Run Gemini CLI in SDK query mode (non-interactive):

```bash
gemini -p "$(cat /tmp/gemini_prompt.txt)"
```

4) Return the result:
   - First, Gemini output as-is (in a fenced block)
   - Then 5–10 lines of your synthesis: what's important, what to do next

5) Delete the temporary file:

```bash
rm /tmp/gemini_prompt.txt
```
