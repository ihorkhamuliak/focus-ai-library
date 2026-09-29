# Saving tokens in Claude Code, measured

[Українська](../token-economy.md) | **English**

The main rule: **measure first, then pull the lever.** Most "save 60-90%" tips were measured on someone else's workload.
On yours they may give 0.25%.

## What helped us most

Starting a session with the rule "read CLAUDE.md, the Dashboard and the project's state file" cost **~122,000 tokens**,
and that context then rides along in every step. Two files out of 177 made up 45% of the spend.

What we did:
1. **A brief instead of reading files.** A `SessionStart` hook outputs what's fresh in the key files within a character budget,
   plus **a map of the rest with line numbers**. The agent then loads the rest precisely (`Read` with `offset`).
2. **A guard against reading in full.** A `PreToolUse` hook refuses `Read` on files over 100 KB without `offset`/`limit`.
   Measured: `Read` accounted for 56% of everything tools put into context.

Ready code: [claude-context-kit](https://github.com/ihorkhamuliak/claude-context-kit).

## The trap we missed for 17 days

**Hook output over 10,000 characters is moved to a file by the harness, and the session gets only a 2 KB preview.** Our brief
weighed 57-98 KB, so for 31 sessions in a row it never arrived at all. The test was green because it checked the script's
output, not what reached the session.
Lesson: delivery is proven by the receiver. Start a new session and ask it to quote a line from deep inside the brief.

## What we tested and did NOT take

| Tip | Measured on us | Why not |
|---|---|---|
| [RTK](https://github.com/rtk-ai/rtk), auto-hook on all commands | 0.25% of the bill | 56% of our commands go through `ssh`, which RTK doesn't know. If you run lots of local `find`/`grep`, it may give you more |
| "Caveman", telegraph-style replies | not measured | hurts text quality; savings can go negative (you rewrite) |
| Scout subagents on a cheaper model | not our workload | useful when searching a big codebase is what hurts |
| Pruning memory | | memory is exactly what catches recurring mistakes |

## Where to look for your own savings

| Area | How to measure | Lever |
|---|---|---|
| Session start | size of what loads before the first message | brief + map, budgets under the 10k cap |
| Reading files in full | `Read`'s share of context | guard > 100 KB, reading by line numbers |
| New MCPs, skills, hooks | `/context` in an empty session | turn off what you don't need: every tool's description rides in every session |
| Images | count by pixels (w×h/750), not by base64 length | extract text from PDFs as text, shrink screenshots |
| Your bots on the API | the provider's Usage API, not your own counter | cache the shared prompt prefix, filter before calling the model |

🔴 **Don't cut where results get worse:** answer quality, checks and memory aren't worth a few cents.
