# Day plan: Todoist + Claude Code + a desktop widget

[Українська](../day-plan-todoist.md) | **English**

The problem was simple: every morning I asked the AI "what's the plan for today?" instead of just doing it. The plan
lived in a table in my notes that only the laptop could see, and the AI didn't even read that far at session start.

Code: [ihorkhamuliak/todoist-plan](https://github.com/ihorkhamuliak/todoist-plan) (Python, MIT, tests).

## How it fits together

- **Todoist knows "what and when"**: dated tasks, repeats, priorities. Add and close from phone or PC.
- **Notes know "why and how"**: the task description has 1-2 lines and a link to the note section with the context.
- **Claude gets a short slice at session start**: overdue, today, next 3 days and what was closed in 3 days.
  It creates and closes tasks itself through a small CLI.
- **A Rainmeter widget** shows the same plan on the desktop. **Win+D** shows it instantly, a click opens Todoist.
- **Personal tasks** (bills, habits, errands) fold into one `БАЗОВЕ: …` line: visible, but not pushing work down.

## What it gave us

- One source of truth instead of a notes table nobody saw away from the laptop.
- Repeats remind by themselves. The weekly backup used to "warn nobody": a 13-day gap was found by accident.
- **Closed-task journal:** on every widget refresh the code appends what was closed and when to the notes. The notes
  know what got done even after a week without a session. The meaning ("the lead replied, change the status")
  is updated by Claude at session start.

## What we deliberately did not do

| Idea | Why not |
|---|---|
| A nightly `claude -p` that edits notes from the journal | unsupervised edits, conflicts with an open session, money every day |
| A Todoist MCP server | its tools sit in the context of every session; a CLI costs nothing until called |
| A checklist as subtasks under a repeating task | a closed subtask does not reopen when the repeat moves to its next date |
| AutoHotkey for the toggle | one more third-party binary; Win+D does the job instantly |

## API traps we hit (October 2026)

1. Deadlines (`deadline_date`) are Pro only: Free returns 403 `PREMIUM_ONLY`. We put the deadline in the description.
2. Todoist cuts `@word` out of a title as a label even with `auto_parse_labels=False`.
3. Fixed-timezone dues come back in UTC: "31 Oct 00:00 Warsaw" = "30 Oct 23:00". Without a shift the task lands a day early.
4. Closing a repeat is not in the completed-tasks list (the repeat moves). The activity log sees it, a few seconds late.
5. `getpass` in the VS Code terminal ignores Ctrl+V and silently saves an empty token. We read the token from the clipboard.
6. Check every filter and detector on a known defect. Our "overdue repeat" test failed first, and that is what proved it catches anything.

## Who it's for

People who use Claude Code daily and keep tasks in notes or in their head. Setup takes ~15 minutes, see the repo README.

## When you don't need it

- If your plan already lives in one place you see on both phone and PC.
- On a Mac: Rainmeter is Windows only (the CLI and the brief work anywhere).
- If you have up to 5 tasks a day: a calendar is enough.
