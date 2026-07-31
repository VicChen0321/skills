---
name: to-code-review
description: Review a ticket's implementation for reuse, simplification, efficiency, and clarity using the quality-reviewer agent, then apply fixes directly. Use when a ticket's status is In Review, or the user asks to clean up or review recent changes. Does not check correctness - that stays a separate concern.
---

# to-code-review

## Purpose

Once `to-implement` finishes a ticket, or on any recent diff the user points at, review it for reuse, simplification, efficiency, and clarity only, using a dedicated `quality-reviewer` subagent, and apply the agreed fixes directly.
This is deliberately not a correctness or spec-compliance review; that stays out of scope for now.

## When to use

A ticket's status is `In Review`, or the user asks to clean up or review recent changes.

## Process

1. Determine the diff: the ticket's changes since `to-implement` started (`git diff` against the ticket's base commit), or the current working-tree changes if there's no ticket context.
2. Spawn the `quality-reviewer` agent with that diff.
3. The agent applies its own fixes directly; it has edit access for exactly this reason, so you don't need to re-apply anything it already did.
4. Re-run the full test suite after the agent's fixes are applied. If a test that passed before the fixes now fails, revert that specific fix rather than the whole diff.
5. Update the ticket's `Status` to `Done` and summarize what changed.

## Handoff

None yet. `to-test` and `to-ship` are future work.
