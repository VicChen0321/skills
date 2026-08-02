---
name: to-code-review
description: Review a ticket's implementation on two independent axes - code quality and spec compliance - then commit the work. Use when a ticket's status is In Review, or the user asks to review recent changes.
---

# to-code-review

## Purpose

Once `to-implement` finishes a ticket, or on any recent diff the user points at, review it on two independent axes at once: whether the code is well-written (`quality-reviewer`), and whether it actually does what the ticket and spec asked for (`spec-reviewer`).
Both report findings only; neither edits anything.
Keeping the axes separate means a change that's clean but wrong, or correct but messy, doesn't get lost under the other kind of finding.

## When to use

A ticket's status is `In Review`, or the user asks to review recent changes.

## Process

1. Determine the diff: the ticket's changes since `to-implement` started (`git diff` against the ticket's base commit), or the current working-tree changes if there's no ticket context.
2. Identify the spec source for `spec-reviewer`: the ticket's linked spec (file or issue, per `docs/ai-sdlc/issue-tracker.md` - default local markdown if that file doesn't exist, see `setup-issue-tracker`) and its own Acceptance Criteria checklist.
3. Run `quality-reviewer` and `spec-reviewer` against the same diff, using [`../../../agents/quality-reviewer.md`](../../../agents/quality-reviewer.md) and [`../../../agents/spec-reviewer.md`](../../../agents/spec-reviewer.md) as their exact rubrics.
   - If the harness supports subagents, spawn both reviewers in parallel.
   - Otherwise, conduct two sequential passes yourself.
     Read only the relevant rubric before each pass, keep the findings separate, and do not edit files during either pass.
4. Present both reports under separate headings, unedited. Don't cross-rank or merge them - a change can have serious findings on one axis and none on the other, and collapsing them into one list hides that.
5. Commit the finished work to the current branch, with a message referencing the ticket. The commit happens regardless of what the reports found; findings are advisory, not a gate.
6. Mark the ticket Done, with both reports attached (or a pointer to them) so any follow-up work stays visible:
   - **Local**: update the ticket file's `Status` to `Done`.
   - **GitHub**: `gh issue close <n> --comment "..."` with both reports in the closing comment.
   - **Other**: follow the workflow described in `docs/ai-sdlc/issue-tracker.md`.

## Handoff

None yet. `to-test` and `to-ship` are future work.
