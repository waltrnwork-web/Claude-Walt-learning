# Claude-Walt-learning

Test repo for Claude Code experiments.

## Purpose
Track every Claude Code session result so patterns become clear.  
Record what worked, what needs more work, and what failed — with a short reason.

This repository is a controlled playground for evaluating Claude Code on real tasks.  
Each experiment is intentionally small, recorded, and judged with a simple three-way result so that progress stays visible and repeatable.

The goal is not to build production software here. It is to learn how Claude Code behaves under different prompts, models, permission modes, and task sizes — then capture that knowledge in a form that can be reviewed later.

## Result Categories

| Category | Meaning | When to use |
|----------|---------|-------------|
| **Pass** | Task completed as requested, output is usable | Code runs, requirements met, no major issues. Ready to use or build on. |
| **Hold** | Partial success or needs review | Works but has gaps, unclear, or needs a human decision before it can be trusted. |
| **Fail** | Did not meet the goal | Broken, wrong approach, or requires a full restart. Continuing from the current state would waste time. |

### Rationale
Categorizing results this way keeps experiments cheap and useful.  
A Pass can be built on. A Hold gets a quick decision. A Fail stops wasted effort.  
Using only three categories forces a clear decision at the end of every session and prevents vague “it kind of worked” notes that are hard to act on later.  
Over time the log shows which kinds of prompts and tasks Claude handles reliably.

## Improvement Options

When a result is Hold or Fail, pick one of these next moves:

1. **Tighten the prompt**  
   Add the missing constraint, an example of the desired output, or an explicit “do not” rule.  
   Use this when Claude understood the general idea but drifted on details, format, or scope.  
   *Rationale:* Most Hold and Fail results come from underspecified requests rather than model limits. A tighter prompt is usually the fastest path to a Pass.

2. **Break the task smaller**  
   Split the original request into two or three sequential steps and re-run only the first one.  
   Use this when the task was too large, touched too many files, or mixed planning with implementation.  
   *Rationale:* Claude Code is more reliable on focused, single-purpose changes. Smaller steps also make it easier to see exactly where a result stopped being correct.

3. **Switch model or permission mode**  
   Move to a higher-effort Opus setting, or change from Accept edits to Plan mode (or the reverse).  
   Use this when the approach itself looks wrong, the model appears to be taking shortcuts, or you want a safer review step before any files are changed.  
   *Rationale:* Different models and modes trade speed, thoroughness, and autonomy. Changing them is appropriate when the problem is not the prompt wording but the way the agent is allowed to work.

## Suggested Workflow
1. Write a short, specific task.  
2. Run it in Claude Code.  
3. Immediately label the result Pass, Hold, or Fail and note the reason.  
4. If it is not a Pass, choose one of the three options above and record which one you picked.  
5. Repeat.  

Over a series of sessions the log becomes a practical guide to what kinds of tasks and instructions work best with Claude Code for this account.

## Log format (optional)
