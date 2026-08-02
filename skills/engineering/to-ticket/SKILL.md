---
name: to-ticket
description: Decompose an approved spec into tracer-bullet tickets - narrow, independently demoable vertical slices with declared blocking order.
disable-model-invocation: true
---

# to-ticket

## Purpose

Decompose an approved spec into tracer-bullet tickets: each one a narrow but complete vertical slice through every layer it touches (schema, API, UI, tests, whatever the spec's stack implies), independently demoable, and sized to fit in a single `to-implement` session.
Tickets are lightweight: a title, a description, and acceptance criteria, not full step-by-step execution plans.
Turning a ticket into working code is `to-implement`'s job, not this skill's.

## When to use

A spec exists, either just written by `to-spec` or an existing file the user points at, and the user wants it broken into actionable work items.

## Process

1. Check `docs/ai-sdlc/issue-tracker.md` for the backend (default to local markdown if it doesn't exist - see `setup-issue-tracker`), then read the spec: for local, a path the user provides or the most recent file in `docs/ai-sdlc/specs/`; for GitHub, the issue number the user provides or the most recent open issue labelled `spec` (`gh issue view <n> --comments`).
2. Slice the spec into vertical, demoable units, not horizontal layers. A ticket should never be "write the API" with a separate ticket for "write the UI" for the same capability; it should be "user can do X end to end," even in a minimal form. Vertical slices stay shippable on their own; horizontal ones don't demo until every layer is done.
3. Draft the ticket list and show it to the user for adjustment (splitting, merging, reordering) before publishing anything.
4. Publish each ticket per the backend:
   - **Local** (default): one file per ticket, `docs/ai-sdlc/tickets/<spec-slug>/NN-<ticket-slug>.md`, numbered in dependency order.
   - **GitHub**: one issue per ticket, `gh issue create --label ticket --title "[NN] <title>" --body "..."`, with `Spec: #<spec-issue-number>` and `Blocked by: #<n>, #<n>` in the body.
   - **Other**: follow the workflow described in `docs/ai-sdlc/issue-tracker.md`.
5. For local only, write an index file, `docs/ai-sdlc/tickets/<spec-slug>/README.md`, listing every ticket with its status, so the "frontier" of unblocked, ready-to-start tickets is visible at a glance. GitHub needs no separate index - `gh issue list --label ticket --state open` is always the live one.
6. Self-review: every goal in the spec maps to at least one ticket, no two tickets claim the same scope, and the blocking graph has no cycles.

## Ticket file template (local backend)

```markdown
# [NN] - [Ticket Title]

**Spec:** ../../specs/<spec-file>.md
**Blocked by:** [other ticket numbers/titles, or "None - can start immediately"]
**Status:** Todo

**What to build:** [the end-to-end user-observable behavior, not implementation steps]

## Acceptance Criteria

- [ ]
- [ ]
```

The GitHub backend carries the same content (spec link, blocked-by, what-to-build, acceptance criteria) in the issue body instead - see `setup-issue-tracker`'s `issue-tracker-github.md` for the exact conventions.

## Handoff

Once tickets are written, suggest invoking `to-implement` on the first unblocked ticket.
