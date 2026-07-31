# AI-SDLC Skills Plugin: Repo Structure & Initial Skills

**Date:** 2026-07-31
**Status:** Draft

## Motivation

This repository organizes Vic's personal AI agent skills for Claude Code.
It is installed as a Claude Code plugin, distributed via a self-hosted plugin marketplace.
The skills follow the Software Development Life Cycle (SDLC), adapted for an AI-assisted workflow (AI-SDLC): idea capture, planning, implementation, review, testing, and release each get their own skill.
This spec covers the repo/plugin scaffolding and the first five skills: the pipeline `to-spec` -> `to-ticket` -> `to-implement` -> `to-code-review`, plus `tdd`, a technique skill that `to-implement` drives at each seam.

`to-ticket` uses a tracer-bullet ticket format, `tdd` uses a seam-based red-green loop, and refactoring belongs to `to-code-review` rather than the red-green cycle itself.

## Goals

- Make this repo installable as a Claude Code plugin via the marketplace flow (`/plugin marketplace add`, `/plugin install`).
- Organize skills by SDLC phase so the collection can grow without restructuring later.
- Ship `to-spec` (idea -> written spec), `to-ticket` (spec -> backlog tickets), `to-implement` (ticket -> code + tests), and `to-code-review` (code -> improved code) as the first working chain.
- Ship `tdd` as the technique skill `to-implement` drives at each seam: one failing test, minimal code to pass, no refactoring mixed in.
- Give `to-code-review` its own dedicated subagent, `quality-reviewer`, built into this plugin rather than depending on another installed plugin.
- Keep the repo shareable: a stranger should be able to clone it, install it, and understand what each skill does from the README alone.

## Non-goals (for this iteration)

- No external tracker integration (GitHub Issues, Jira, Linear). Tickets are local markdown files only.
- No `to-test` / `to-ship` skills yet. The structure anticipates them; this spec does not design them.
- `to-code-review` does not check correctness or logic bugs. It is scoped to reuse, simplification, efficiency, and clarity only. Correctness review remains a separate, later concern.
- No CHANGELOG.md. Git history is the changelog at this repo size.
- No cross-machine sync mechanism beyond normal git clone + plugin install.

## Repo & Plugin Structure

The repo root is also the plugin root and the marketplace root, the same pattern the `superpowers` plugin uses (verified by inspecting its installed `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` on this machine).

```
skills/                                    # repo root = plugin root
├── .claude-plugin/
│   ├── plugin.json                        # plugin manifest
│   └── marketplace.json                   # self-referencing marketplace catalog
├── skills/
│   └── engineering/
│       ├── to-spec/
│       │   └── SKILL.md
│       ├── to-ticket/
│       │   └── SKILL.md
│       ├── tdd/
│       │   └── SKILL.md
│       ├── to-implement/
│       │   └── SKILL.md
│       ├── to-code-review/
│       │   └── SKILL.md
│       └── (future) to-test/, to-ship/
├── agents/
│   └── quality-reviewer.md                # subagent used by to-code-review
├── docs/
│   └── specs/                             # design docs for this repo itself
├── README.md
└── LICENSE
```

Skills live under a phase category, `skills/engineering/`, rather than as flat top-level entries.
This anticipates future categories (for example a `research/` or `ops/` grouping) without requiring a rename of existing skills.

Claude Code's default plugin loader only scans `skills/<name>/SKILL.md` one level deep; it does not recurse into subdirectories automatically.
To make skills inside `skills/engineering/` discoverable, `plugin.json` declares an explicit extra scan root:

```json
"skills": ["./skills/engineering/"]
```

This was confirmed against the current Claude Code plugin reference docs: the `skills` manifest field *adds to* the default `skills/` scan (it does not replace it), and each listed directory is scanned for immediate `<name>/SKILL.md` children.
Adding a future category means creating `skills/<category>/<skill-name>/SKILL.md` and appending one line to this array, for example `"./skills/research/"`.

### plugin.json

```json
{
  "name": "ai-sdlc",
  "description": "AI-assisted SDLC skills for Claude Code: spec, ticket, TDD, implement, and code review, with test/ship stages to come",
  "version": "0.1.0",
  "author": { "name": "Vic Chen", "email": "victor72441299@gmail.com" },
  "homepage": "https://github.com/<your-github-username>/skills",
  "repository": "https://github.com/<your-github-username>/skills",
  "license": "MIT",
  "keywords": ["skills", "sdlc", "spec", "ticket", "tdd", "code-review", "planning", "workflow"],
  "skills": ["./skills/engineering/"]
}
```

### marketplace.json

```json
{
  "name": "ai-sdlc-marketplace",
  "description": "Marketplace for Vic's AI-SDLC skills plugin",
  "owner": { "name": "Vic Chen", "email": "victor72441299@gmail.com" },
  "plugins": [
    {
      "name": "ai-sdlc",
      "description": "AI-assisted SDLC skills for Claude Code",
      "version": "0.1.0",
      "source": "./",
      "author": { "name": "Vic Chen", "email": "victor72441299@gmail.com" }
    }
  ]
}
```

The `<your-github-username>` placeholders get filled in once the repo has a GitHub remote.

### Install flow

Once pushed to GitHub:

```
/plugin marketplace add <you>/skills
/plugin install ai-sdlc
```

Local development and edits to `SKILL.md` files take effect immediately in the current Claude Code session.
Changes to other plugin components (manifests, hooks) need `/reload-plugins` or a restart.

## Skill: to-spec

**Purpose:** turn a raw idea or feature request into a written, approved spec, through a short interview rather than a one-shot guess.

**Trigger context:** the user describes a new feature, a change, or an idea and hasn't yet written down what "done" looks like. Phrases like "let's build X," "I want to add Y," "spec out Z."

**Process:**

1. Explore the target project's context (README, recent commits, relevant existing code) before asking anything.
2. Ask clarifying questions one at a time, multiple-choice where it fits, covering purpose, constraints, and success criteria.
3. Propose 2-3 approaches with tradeoffs, lead with a recommendation.
4. Present the design in sections scaled to their complexity, get approval on each section before moving to the next.
5. Write the approved spec to `docs/ai-sdlc/specs/YYYY-MM-DD-<topic>-spec.md` in the target project (not in this plugin repo).
6. Run a self-review pass: no placeholders/TBDs, no internal contradictions, scope is right for a single `to-ticket` pass, no requirement is ambiguous.
7. Ask the user to review the written spec file before moving on.

**Spec document template:**

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

**Handoff:** once the user approves the written spec, `to-spec` suggests invoking `to-ticket` next.

## Skill: to-ticket

**Purpose:** decompose an approved spec into tracer-bullet tickets: each one a narrow but complete vertical slice through every layer it touches (schema, API, UI, tests, whatever the spec's stack implies), independently demoable, and sized to fit in a single `to-implement` session.
Tickets are lightweight: title, description, and acceptance criteria, not full step-by-step execution plans.
Turning a ticket into working code is `to-implement`'s job, not this skill's.

**Trigger context:** a spec exists (just written by `to-spec`, or an existing file the user points at) and the user wants it broken into actionable work items.

**Process:**

1. Read the spec, either a path the user provides or the most recent file in `docs/ai-sdlc/specs/`.
2. Slice the spec into vertical, demoable units, not horizontal layers. A ticket should never be "write the API" with a separate ticket for "write the UI" for the same capability; it should be "user can do X end to end," even in a minimal form.
3. Draft the ticket list and show it to the user for adjustment (splitting, merging, reordering) before writing files.
4. Write each ticket to its own file: `docs/ai-sdlc/tickets/<spec-slug>/NN-<ticket-slug>.md`, numbered in dependency order.
5. Write an index file, `docs/ai-sdlc/tickets/<spec-slug>/README.md`, listing every ticket with its status, so the "frontier" of unblocked, ready-to-start tickets is visible at a glance.
6. Self-review: every goal in the spec maps to at least one ticket, no two tickets claim the same scope, and the blocking graph has no cycles.

**Ticket file template:**

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

**Handoff:** once tickets are written, `to-ticket` suggests invoking `to-implement` on the first unblocked ticket.

## Skill: tdd

**Purpose:** drive a single red-green cycle at a named seam: one failing test, then the minimal code to pass it.
Refactoring is deliberately excluded here; it happens in `to-code-review`, once the code is green.
This is a technique skill, not a pipeline stage: it doesn't produce its own artifact in the `to-<noun>` sense, it's the loop another skill drives.

**Trigger context:** invoked by `to-implement` at each seam of a ticket. Also directly invocable by the user for any ad hoc test-first work outside the ticket pipeline.

**Process:**

1. Confirm the seam: the public interface or observable behavior boundary under test. If it isn't already obvious from the caller's context, ask: "What's the public interface, and which seam should we test?"
2. Write one failing test at that seam. It should read like a specification of behavior (for example "user can check out with a valid cart"), not assert on internal implementation details, and its expected value must not be derived from the same logic as the code under test (no tautological assertions).
3. Run the test, confirm it fails for the expected reason, not an unrelated error.
4. Write the minimal code that makes the test pass. Nothing extra: no speculative generalization, no unrequested refactor.
5. Run the test again, confirm it passes.
6. Repeat one seam at a time. Never write all the tests for a ticket before implementing any of them (horizontal slicing); finish one vertical seam fully before starting the next.

**Anti-patterns to reject:**

- Implementation-coupled tests that break on refactor even when behavior is unchanged.
- Tautological assertions.
- Horizontal slicing.
- Sneaking cleanup or refactoring into a red-green cycle. Note it and leave it for `to-code-review`.

**Handoff:** control returns to whichever skill invoked it (usually `to-implement`) once the seam is green.

## Skill: to-implement

**Purpose:** implement a ticket's described behavior as working code, driving `tdd` at each seam, then handing off to `to-code-review` once everything is green.

**Trigger context:** the user points at a ticket file, or says "implement ticket NN," "let's build this ticket."

**Process:**

1. Read the ticket file. If none is specified and more than one ticket is unblocked (`Status: Todo` with every `Blocked by` entry already `Done`), list them and ask which to implement.
2. Read the linked spec for context the ticket alone doesn't carry: constraints, terminology, adjacent components.
3. Break the ticket's "What to build" and acceptance criteria into seams: the public interfaces or observable behaviors that need coverage.
4. For each seam, invoke the `tdd` skill to drive its red-green cycle. Do not write implementation code outside of a `tdd` cycle.
5. Once all seams are green, run the full test suite, not just the new tests, to catch regressions.
6. Update the ticket file's `Status` to `In Review` and note which files were touched.
7. Hand off to `to-code-review`.

**Handoff:** `to-implement` always ends by invoking `to-code-review` on the changes it just made, since the code is green but not yet cleaned up.

## Skill: to-code-review

**Purpose:** once `to-implement` finishes a ticket (or on any recent diff the user points at), review it for reuse, simplification, efficiency, and clarity only, using a dedicated `quality-reviewer` subagent, and apply the agreed fixes directly.
This is deliberately not a correctness or spec-compliance review; that stays out of scope for now (see Non-goals).

**Trigger context:** a ticket's status is `In Review`, or the user asks to clean up or review recent changes.

**Process:**

1. Determine the diff: the ticket's changes since `to-implement` started (`git diff` against the ticket's base commit), or the current working-tree changes if there's no ticket context.
2. Spawn the `quality-reviewer` agent (see below) with that diff.
3. The agent applies its own fixes directly; it has edit access for exactly this reason.
4. Re-run the full test suite after fixes are applied. If a test that passed before the fixes now fails, revert that specific fix rather than the whole diff.
5. Update the ticket's `Status` to `Done` and summarize what changed.

**Handoff:** none yet. `to-test` and `to-ship` are future work (see Roadmap).

## Agent: quality-reviewer

A plugin-owned subagent at `agents/quality-reviewer.md`, invoked only by `to-code-review`.
Self-contained so this plugin doesn't depend on any other installed plugin's agents (for example the separately-installed `code-simplifier` plugin, which this deliberately does not call).

**Scope:** flags and directly fixes exactly the following, and nothing else:

- Reuse: duplicated code, near-duplicate logic that should share an implementation.
- Simplification: unnecessary complexity, dead code, over-engineered abstractions for what the code actually needs.
- Efficiency: obviously wasteful operations (redundant passes, avoidable allocations), not micro-optimization.
- Clarity: naming, structure, and the classic code-smell catalog from Fowler's *Refactoring*: Mysterious Name, Duplicated Code, Feature Envy, Data Clumps, Primitive Obsession, Repeated Switches, Shotgun Surgery, Divergent Change, Speculative Generality, Message Chains, Middle Man, Refused Bequest.

**Explicitly out of scope:** correctness bugs, logic errors, missing edge-case handling, and whether the implementation actually satisfies the ticket's acceptance criteria.
Those need a different reviewer with different tools; flagging them here would blur the boundary `to-implement` and `to-code-review` already draw between "green" and "clean."

**Frontmatter** (plugin agent schema, confirmed against the current Claude Code plugin reference docs):

```markdown
---
name: quality-reviewer
description: Reviews a code diff for reuse, simplification, efficiency, and clarity only, not correctness. Invoked by to-code-review after to-implement finishes a ticket.
model: sonnet
tools: Read, Grep, Glob, Edit, Bash
---
```

## Roadmap

Pipeline-stage skills follow a `to-<noun>` naming convention, each name describing the artifact the skill produces from the previous stage's output.
`tdd` breaks that convention on purpose: it's a technique another skill drives, not a stage that hands off an artifact on its own.

| Skill | Input -> Output | Status |
|---|---|---|
| `to-spec` | idea -> spec | This iteration |
| `to-ticket` | spec -> tickets | This iteration |
| `tdd` | (driven by to-implement, per seam) | This iteration |
| `to-implement` | ticket -> code + tests | This iteration |
| `to-code-review` | code -> improved code | This iteration |
| `to-test` | code -> verification/test coverage | Future |
| `to-ship` | reviewed code -> release/deploy | Future |

Future skills also live under `skills/engineering/`, with `plugin.json`'s `skills` array already pointing at that directory, so adding them requires no manifest changes beyond version bumps.

## Testing / Verification Plan

- Confirm `/plugin marketplace add` and `/plugin install ai-sdlc` succeed from a local path before pushing to GitHub (`/plugin marketplace add ./` from the repo root, or the equivalent local-path flow).
- Confirm all five skills and the `quality-reviewer` agent show up under `/plugin` details and can be invoked by name.
- Run the full chain once against a throwaway sample project: `to-spec` on a small fake feature, `to-ticket` on the resulting spec, `to-implement` on the first ticket (verifying it actually drives `tdd` per seam rather than writing code directly), then `to-code-review` (verifying `quality-reviewer` applies fixes and the test suite still passes afterward).
- Confirm `to-code-review` does not flag or fix correctness issues seeded into the sample project on purpose, only reuse/simplification/efficiency/clarity ones - this is the scope boundary the design depends on.
