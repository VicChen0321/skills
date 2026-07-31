---
name: tdd
description: Drive one red-green cycle at a single named seam - one failing test, then the minimal code to pass it, no refactoring mixed in. Use for any test-first implementation work, standalone or when to-implement invokes it per seam.
---

# tdd

## Purpose

Drive a single red-green cycle at a named seam: one failing test, then the minimal code to pass it.
Refactoring is deliberately excluded here; it happens in `to-code-review`, once the code is green.
This is a technique skill, not a pipeline stage: it doesn't produce its own artifact the way `to-spec` or `to-ticket` do, it's the loop another skill drives, one seam at a time.

## When to use

Invoked by `to-implement` at each seam of a ticket.
Also directly invocable for any ad hoc test-first work outside the ticket pipeline.

## Process

1. Confirm the seam: the public interface or observable behavior boundary under test. If it isn't already obvious from the caller's context, ask: "What's the public interface, and which seam should we test?"
2. Write one failing test at that seam. It should read like a specification of behavior (for example "user can check out with a valid cart"), not assert on internal implementation details, and its expected value must not be derived from the same logic as the code under test - that's a tautological assertion, and it can never fail.
3. Run the test. Confirm it fails for the expected reason, not an unrelated error like a typo or a missing import.
4. Write the minimal code that makes the test pass. Nothing extra: no speculative generalization, no unrequested refactor. Minimal means minimal, even if a "better" version is obvious.
5. Run the test again. Confirm it passes.
6. Repeat one seam at a time. Never write all the tests for a ticket before implementing any of them - that's horizontal slicing, and it means nothing works until everything is done. Finish one vertical seam fully before starting the next.

## Anti-patterns to reject

- Implementation-coupled tests that break on refactor even when behavior is unchanged.
- Tautological assertions.
- Horizontal slicing.
- Sneaking cleanup or refactoring into a red-green cycle. Notice it, name it, and leave it for `to-code-review` instead of doing it here.

## Handoff

Control returns to whichever skill invoked this one, usually `to-implement`, once the seam is green.
