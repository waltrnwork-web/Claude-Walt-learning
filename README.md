# Claude-Walt-learning

## Extended Description

This repository is a controlled playground for evaluating Claude Code on real tasks.  
Each experiment is intentionally small, recorded, and judged with a simple three-way result so that progress stays visible and repeatable.

The goal is not to build production software here. It is to learn how Claude Code behaves under different prompts, models, permission modes, and task sizes — then capture that knowledge in a form that can be reviewed later.

### Why Pass / Hold / Fail

- **Pass** means the output is ready to use or build on. No further work is required for this specific task.  
- **Hold** means the output is directionally correct but incomplete, uncertain, or needs a human decision before it can be trusted. It is not a failure; it is a pause for judgment.  
- **Fail** means the attempt missed the goal badly enough that continuing from the current state would waste time. A Fail is useful because it clearly marks what not to repeat.

Using only three categories forces a clear decision at the end of every session and prevents vague “it kind of worked” notes that are hard to act on later.

### How the three improvement options are meant to be used

1. **Tighten the prompt**  
   Add the missing constraint, an example of the desired output, or an explicit “do not” rule.  
   Use this when Claude understood the general idea but drifted on details, format, or scope.  
   Rationale: Most Hold and Fail results come from underspecified requests rather than model limits. A tighter prompt is usually the fastest path to a Pass.

2. **Break the task smaller**  
   Split the original request into two or three sequential steps and re-run only the first one.  
   Use this when the task was too large, touched too many files, or mixed planning with implementation.  
   Rationale: Claude Code is more reliable on focused, single-purpose changes. Smaller steps also make it easier to see exactly where a result stopped being correct.

3. **Switch model or permission mode**  
   Move to a higher-effort Opus setting, or change from Accept edits to Plan mode (or the reverse).  
   Use this when the approach itself looks wrong, the model appears to be taking shortcuts, or you want a safer review step before any files are changed.  
   Rationale: Different models and modes trade speed, thoroughness, and autonomy. Changing them is appropriate when the problem is not the prompt wording but the way the agent is allowed to work.

### Suggested workflow
1. Write a short, specific task.  
2. Run it in Claude Code.  
3. Immediately label the result Pass, Hold, or Fail and note the reason.  
4. If it is not a Pass, choose one of the three options above and record which one you picked.  
5. Repeat.  

Over a series of sessions the log becomes a practical guide to what kinds of tasks and instructions work best with Claude Code for this account.
