---
name: log-result
description: Log a Claude Code experiment result to RESULTS.md in the repo root. Use when the user says things like "log this result", "record the outcome", "mark this as pass/hold/fail", "add this to the log", or otherwise asks to record how an experiment went. Captures a short task name, a result (Pass, Hold, or Fail), a one-sentence summary of what happened, and an optional next step, then appends a dated entry. Only for logging results, not for analyzing or fixing them.
---

# Log Result

Append one experiment result to `RESULTS.md` in the repository root.

## 1. Gather the three required fields

- **Task name**: a short name for the task (a few words).
- **Result**: exactly one of `Pass`, `Hold`, or `Fail`. Normalize the case (for example "pass" becomes `Pass`). If the user gives anything else, ask them to pick one of the three.
- **What happened**: one sentence. If the user gives more, condense it to one sentence that keeps their meaning.

Optional:
- **Next**: which improvement option to try next. Include it only if the user gives one.

Use whatever the user has already said, including the current conversation (for example, the task you just worked on), to fill in the fields. If a required field is still missing or unclear, ask for only the missing fields in a single short question. Don't guess the result.

## 2. Get today's date

Run `date +%F` to get the date as `YYYY-MM-DD`. Don't rely on memory for the date.

## 3. Append the entry

Use this exact format, with an en dash (`–`) between the date and the task name:

```markdown
### YYYY-MM-DD – short task name
- Result: Pass | Hold | Fail
- What happened: one sentence
- Next: improvement option
```

- Leave out the `- Next:` line entirely when no next step was given.
- `Result` holds only the single chosen value, for example `- Result: Hold`.

Steps:
1. If `RESULTS.md` doesn't exist in the repo root, create it with a `# Results` heading followed by a blank line.
2. Append the new entry at the end of the file, with one blank line separating it from the previous content.
3. Never edit or reorder existing entries.

## 4. Confirm

Show the user the entry you added, in a short reply. Don't commit or push unless the user asks.
