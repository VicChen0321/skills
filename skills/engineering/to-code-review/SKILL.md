---
name: to-code-review
description: Review a ticket's implementation on two independent axes - code quality and spec compliance - using parallel subagents, then commit the work. Use when a ticket's status is In Review, or the user asks to review recent changes.
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
2. Identify the spec source for `spec-reviewer`: the ticket's linked spec file and its own Acceptance Criteria checklist.
3. Spawn `quality-reviewer` and `spec-reviewer` in parallel against the same diff.
4. Present both reports under separate headings, unedited. Don't cross-rank or merge them - a change can have serious findings on one axis and none on the other, and collapsing them into one list hides that.
5. Commit the finished work to the current branch, with a message referencing the ticket. The commit happens regardless of what the reports found; findings are advisory, not a gate.
6. Update the ticket's `Status` to `Done`, and include both reports, or a pointer to them, so any follow-up work stays visible.

## Handoff

None yet. `to-test` and `to-ship` are future work.
