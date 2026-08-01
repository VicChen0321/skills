---
name: to-implement
description: Implement a ticket's described behavior as working code, driving the tdd skill at each seam, then handing off to to-code-review.
disable-model-invocation: true
---

# to-implement

## Purpose

Implement a ticket's described behavior as working code, driving `tdd` at each seam, then handing off to `to-code-review` once everything is green.

## When to use

The user points at a ticket file, or says "implement ticket NN," "let's build this ticket."

## Process

1. Read the ticket file. If none is specified and more than one ticket is unblocked (`Status: Todo` with every `Blocked by` entry already `Done`), list them and ask which to implement.
2. Read the linked spec for context the ticket alone doesn't carry: constraints, terminology, adjacent components.
3. Break the ticket's "What to build" and acceptance criteria into seams: the public interfaces or observable behaviors that need coverage.
4. For each seam, invoke the `tdd` skill to drive its red-green cycle. Do not write implementation code outside of a `tdd` cycle - if you catch yourself writing code without a failing test driving it, stop and go back through `tdd` for that seam.
5. Once all seams are green, run the full test suite, not just the new tests, to catch regressions the individual seams wouldn't show.
6. Update the ticket file's `Status` to `In Review` and note which files were touched.
7. Hand off to `to-code-review`.

## Handoff

Always end by invoking `to-code-review` on the changes just made, since the code is green but not yet cleaned up.
