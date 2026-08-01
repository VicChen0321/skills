---
name: resolving-merge-conflicts
description: Resolve a git merge or rebase conflict by tracing the intent behind each side, not by mechanically picking one.
disable-model-invocation: true
---

# resolving-merge-conflicts

A conflict marker just means git couldn't reconcile two changes automatically, not that one side is right and the other is wrong.
Before touching a hunk, find out what each side was actually trying to accomplish: read the commit messages on both sides, and the PR or issue behind them if one exists.

## Process

1. Check the current state: which files are conflicted, and what the merge or rebase is actually trying to combine.
2. For each conflicting file, trace intent on both sides before editing anything. What was each change trying to accomplish?
3. Resolve each hunk: keep both intents where they're compatible with each other. Where they genuinely conflict, follow whichever direction the merge is supposed to be moving toward, and note in the commit what got dropped and why.
4. Never invent behavior neither side asked for. If the right resolution isn't clear from either side's intent, ask rather than guess.
5. Run the project's checks (typecheck, tests, formatter) before considering it resolved. A conflict that merges cleanly but breaks the build isn't actually resolved.
6. Stage everything and complete the merge or rebase.

## Anti-patterns

- Picking a side mechanically (always "ours," always "theirs") without checking what either side was trying to do.
- Reaching for `--abort` to escape a hard conflict instead of resolving it.
- Inventing behavior neither side wrote, just to make the conflict go away.
