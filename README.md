# ai-sdlc

AI-assisted SDLC skills for Claude Code.
Each skill is one stage of the software development life cycle, adapted for an AI-assisted workflow: capture an idea, break it into tickets, implement each ticket test-first, then clean it up.

## Install

```
/plugin marketplace add VicChen0321/skills
/plugin install ai-sdlc
```

For local development, load the plugin directly from a working copy for the duration of a session instead:

```
claude --plugin-dir .
```

## Skills

Skills are organized by category, one subdirectory under `skills/` per category, matching the sections below.

### Engineering

| Skill | Input -> Output | Status |
|---|---|---|
| `to-spec` | idea -> spec | This iteration |
| `to-ticket` | spec -> tickets | This iteration |
| `tdd` | (driven by to-implement, per seam) | This iteration |
| `to-implement` | ticket -> code + tests | This iteration |
| `to-code-review` | code -> improved code | This iteration |
| `domain-modeling` | terminology -> CONTEXT.md / ADRs | This iteration |
| `interrogate-with-docs` | plan or design -> sharpened design + docs | This iteration |
| `to-test` | code -> verification/test coverage | Future |
| `to-ship` | reviewed code -> release/deploy | Future |

Pipeline-stage skills follow a `to-<noun>` naming convention, each name describing the artifact the skill produces from the previous stage's output.
`tdd`, `domain-modeling`, and `interrogate-with-docs` are exceptions: technique and utility skills other work draws on, not stages that hand off an artifact to the next stage in sequence.

`to-spec`, `to-ticket`, and `to-implement` are user-invoked only: each starts a phase you decide to kick off by name, not one the model fires on its own partway through a conversation. `tdd`, `domain-modeling`, and `to-code-review` stay model-invoked, since they need to be reachable both directly and from inside another skill's own process.

### to-spec

Turns a raw idea or feature request into a written, approved spec through a short interview.
Explores the target project's context first, asks clarifying questions one at a time, and writes the result to `docs/ai-sdlc/specs/`.
If `interrogate-with-docs` just ran in the same session, skips re-interviewing and drafts straight from what it already settled.

### to-ticket

Decomposes an approved spec into tracer-bullet tickets: narrow, independently demoable vertical slices, each sized to fit a single `to-implement` session.
Each ticket declares what blocks it, so the set of ready-to-start work is always visible.

### tdd

Drives a single red-green cycle at a named seam: one failing test, then the minimal code to pass it.
Reads `CONTEXT.md` first, when it exists, so test names match the project's domain vocabulary; see `tests.md` and `mocking.md` for worked examples.
Refactoring is deliberately excluded here; that's `to-code-review`'s job once the code is green.
This one breaks the `to-<noun>` naming pattern on purpose - it's a technique another skill drives, not a stage that hands off its own artifact.

### to-implement

Implements a ticket's described behavior as working code, driving `tdd` at each seam rather than writing tests or implementation directly.
Runs the full test suite once every seam is green, then hands off to `to-code-review`.

### to-code-review

Reviews a ticket's implementation on two independent axes at once, using two parallel subagents: `quality-reviewer` (reuse, simplification, efficiency, clarity, repo conventions) and `spec-reviewer` (does the diff actually satisfy the ticket's acceptance criteria and spec). Both report findings only; neither edits anything.
Commits the finished work regardless of what the reports found, since findings are advisory, not a merge gate, then updates the ticket to `Done` with both reports attached.

### domain-modeling

Builds and sharpens a project's domain model: challenges terminology conflicts, sharpens vague terms, stress-tests concepts with edge-case scenarios, and cross-references against the actual code.
Writes results inline to `CONTEXT.md` (or a `CONTEXT-MAP.md` plus per-context `CONTEXT.md` files in multi-context repos) and to `docs/adr/`, and only on demand, never batched.

### interrogate-with-docs

Combines `interrogate`'s one-question-at-a-time questioning with `domain-modeling`'s documentation discipline in a single session.
User-invoked only: reach for it over plain `interrogate` when the plan or design being sharpened deserves a lasting record. Hands off to `to-spec` once a shared understanding is confirmed.

### Productivity

| Skill | Purpose | Status |
|---|---|---|
| `writing-great-skills` | craft principles for writing effective skills | This iteration |
| `interrogate` | rigorously question a plan, decision, or idea before acting on it | This iteration |

#### writing-great-skills

Vocabulary and craft principles for writing and reviewing skills: invocation mode, information hierarchy, when to split, pruning discipline, leading words.
User-invoked only - a `GLOSSARY.md` sibling file defines every bolded term once, so `SKILL.md` can use them without re-explaining. Reach for it by name when writing or reviewing a `SKILL.md`.

#### interrogate

Rigorously questions a plan, decision, or idea, one question at a time: facts get researched, not asked; decisions get asked, one at a time, each with a recommended answer; nothing gets acted on until there's a confirmed shared understanding.
Triggers on "grill me," "stress-test this," "poke holes in this," or any request to sense-check thinking before committing to it.

## Design

See `docs/specs/2026-07-31-ai-sdlc-plugin-design.md` for the full design rationale, including why the repo is structured as a single self-referencing plugin and marketplace, and `docs/plans/2026-07-31-ai-sdlc-plugin-implementation-plan.md` for how it was built.

## Acknowledgments

Several skills in this repo take inspiration from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT License), an existing public skills collection covering similar ground. Each skill here is an independent adaptation, not a copy.

## License

MIT, see [LICENSE](LICENSE).
