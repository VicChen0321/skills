# Issue tracker: Local markdown

Specs and tickets live as markdown files inside this project, not in an external tracker.

## Conventions

- **Spec**: `docs/ai-sdlc/specs/YYYY-MM-DD-<topic>-spec.md`, one file, written by `to-spec`.
- **Tickets**: `docs/ai-sdlc/tickets/<spec-slug>/NN-<ticket-slug>.md`, one file per ticket, numbered in dependency order, written by `to-ticket`.
- **Index**: `docs/ai-sdlc/tickets/<spec-slug>/README.md` lists every ticket with its status.
- **Status**: a `Status:` line near the top of each ticket file - `Todo` -> `In Review` (set by `to-implement`) -> `Done` (set by `to-code-review`).
- **Blocking order**: a `Blocked by:` line naming other ticket numbers or titles.

## When a skill says "publish the spec" or "create a ticket"

Write the file at the path above.

## When a skill says "find the relevant ticket"

Read the file directly, or check the spec's `README.md` index for what's unblocked.
