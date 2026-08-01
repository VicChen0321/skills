---
name: to-spec
description: Turn a raw idea or feature request into a written, approved spec through a short interview.
disable-model-invocation: true
---

# to-spec

## Purpose

Turn a raw idea or feature request into a written, approved spec, through a short interview rather than a one-shot guess.
A spec written without asking questions first tends to encode whatever assumptions you happened to make, not what the user actually needs.

## When to use

The user describes a new feature, a change, or an idea, and hasn't yet written down what "done" looks like.
Phrases like "let's build X," "I want to add Y," "spec out Z."

## Process

1. Explore the target project's context before asking anything: read the README, recent commits, and any existing code relevant to the idea. Don't ask the user something the codebase already answers.
2. Ask clarifying questions one at a time. Prefer multiple-choice where it fits, since it's faster to answer than an open-ended question. Cover purpose, constraints, and success criteria before moving on. Skip this step entirely if `interrogate-with-docs` just ran in this session on the same topic - treat what it settled, and anything it wrote to `CONTEXT.md` or an ADR, as already-known context. Only ask about something that's still genuinely unresolved.
3. Propose 2-3 approaches with tradeoffs. Lead with a recommendation and explain why, don't just list options neutrally.
4. Present the design in sections, scaled to their complexity: a couple of sentences for a straightforward section, more for a nuanced one. Get approval on each section before moving to the next, so a wrong turn gets caught early rather than after the whole spec is written.
5. Write the approved spec to `docs/ai-sdlc/specs/YYYY-MM-DD-<topic>-spec.md` in the target project, not in this plugin's own repo.
6. Run a self-review pass on the written file: no placeholders or TBDs, no internal contradictions between sections, the scope is right for a single `to-ticket` pass afterward (if it covers multiple independent subsystems, say so and suggest splitting it), and no requirement is ambiguous enough to be read two ways.
7. Ask the user to review the written spec file before moving on. Wait for their answer; don't assume approval just because you didn't hear an objection to an earlier section.

## Spec document template

```markdown
# [Feature/Change Name]

**Date:** YYYY-MM-DD
**Status:** Draft | Approved

## Problem / Motivation

## Goals

## Non-goals

## Proposed Approach

## Alternatives Considered

## Open Questions / Risks

## Success Criteria
```

## Handoff

Once the user approves the written spec, suggest invoking `to-ticket` next to break it into tickets.
