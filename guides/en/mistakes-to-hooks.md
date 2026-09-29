# Recurring AI mistakes → Claude Code hooks

[Українська](../mistakes-to-hooks.md) | **English**

The problem: you write a rule in `CLAUDE.md` or in memory, the agent knows it, and breaks it anyway. Memory doesn't fire
at the moment of action; a hook does. Below is how we move mistakes from "memory" to "hook".

## The transfer rule

1. A mistake happened **a second time** even though the rule is already in memory → add it to the table.
2. If it can be recognized by a machine (command text, a line in a file) → write a hook (`PreToolUse` for commands and edits,
   `Stop` for the chat reply) and a test: the hook **catches a known violation and lets a clean case through**.
3. If a machine can't recognize it (tone, word choice) → it stays in memory, marked "memory only" in the table.
4. Once a month, look at the trigger log: a hook that never fired is either unnecessary or blind.

## Our table (example)

| Mistake | Times before the hook | What catches it | Mode |
|---|---|---|---|
| Em dashes in texts meant to be sent | regularly | hook on new quoted lines in files + on the last chat message | block in files, request in chat |
| `Co-Authored-By: Claude` in a commit | 3 | hook on `git commit` + `attribution: ""` in `~/.claude/settings.json` | block |
| Push while the local branch is behind the remote | once nearly overwrote a file | hook: `git fetch` and count commits behind | block |
| Force-push without `--force-with-lease` | risk for a parallel session | hook on `push --force` | asks the human |
| Editing n8n directly in the DB (`UPDATE workflow_entity`) without `workflow_history` | 2, one stayed hidden for 2 weeks | hook on the SQL text | block |
| Code with `\n` via heredoc | 2 in a day, prod was down 7 hours | hook on heredoc with escapes | warning |
| Reading a huge file in full | systematically | `PreToolUse` on `Read` without `offset`/`limit` for files > 100 KB | block |
| "All good" without checking that the measurement can see a failure | 3 in one session | not machine-recognizable | memory only |

## Know this up front

- **Off switches.** Each hook checks for a flag file (for example `~/.claude/bash-guard-off`): file exists = hook off,
  no restart needed. Without this, a broken hook will block your work in the middle of the night.
- **Fail-open.** If the hook script crashes, the action is allowed. A bug in the guard doesn't stop the work.
- **The hook reads the command text.** SQL sitting in a file on the server (`psql < f.sql`) is invisible to it.
- **Substring matching = false positives.** Our first hook even blocked `echo "... Co-Authored-By"`.
  We narrowed it to a `git commit` call at the start of the command and added that case to the test.
