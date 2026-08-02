---
name: setup-issue-tracker
description: Configure which issue tracker to-spec, to-ticket, to-implement, and to-code-review write to and read from - GitHub, local markdown, or a freeform description of anything else. Run once per project before first use.
disable-model-invocation: true
---

# setup-issue-tracker

## Purpose

Record, once per project, where specs and tickets live and how their status is tracked, so `to-spec`, `to-ticket`, `to-implement`, and `to-code-review` don't have to assume a single storage convention.
Any project that hasn't run this skill keeps working exactly as before: those four skills default to local markdown under `docs/ai-sdlc/` when no config file is found.

## When to use

Before the first `to-spec` run in a project that wants its specs and tickets to live somewhere other than local markdown - most commonly, a project that already tracks work in GitHub Issues.
Re-run it to switch backends or correct a wrong answer; it overwrites the existing config file after confirming the new contents with the user.

## Process

1. Explore the target project: `git remote -v` to check for a GitHub remote, and whether `docs/ai-sdlc/issue-tracker.md` already exists (a re-run).
2. Ask one question, recommending an answer based on what step 1 found:
   - **GitHub** (recommended when a GitHub remote is present) - specs and tickets become GitHub issues; the `gh` CLI handles every operation.
   - **Local markdown** (recommended otherwise, and it's what every project already does without running this skill) - today's `docs/ai-sdlc/specs/` and `docs/ai-sdlc/tickets/` convention.
   - **Other** - describe the workflow (Jira, Linear, anything else) in one paragraph; recorded as-is, with no seed template.
3. Write `docs/ai-sdlc/issue-tracker.md`:
   - GitHub / Local: start from this skill's matching seed file (`issue-tracker-github.md` / `issue-tracker-local.md`), adjusted for anything project-specific step 1 turned up.
   - Other: write the user's description directly.
4. Confirm the written file with the user, then tell them which skills now read it.

## Handoff

None - this is a one-time setup step other skills read from, not a stage in the pipeline.
