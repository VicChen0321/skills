# ai-sdlc

AI-assisted SDLC skills for Claude Code.
Each skill is one stage of the software development life cycle, adapted for an AI-assisted workflow: capture an idea, break it into tickets, implement each ticket test-first, then clean it up.

## Install

Once this repo has a GitHub remote:

```
/plugin marketplace add <you>/skills
/plugin install ai-sdlc
```

For local development, before pushing anywhere, load the plugin directly from a working copy for the duration of a session:

```
claude --plugin-dir .
```

## Skills

| Skill | Input -> Output | Status |
|---|---|---|
| `to-spec` | idea -> spec | This iteration |
| `to-ticket` | spec -> tickets | This iteration |
| `tdd` | (driven by to-implement, per seam) | This iteration |
| `to-implement` | ticket -> code + tests | This iteration |
| `to-code-review` | code -> improved code | This iteration |
| `to-test` | code -> verification/test coverage | Future |
| `to-ship` | reviewed code -> release/deploy | Future |

Pipeline-stage skills follow a `to-<noun>` naming convention, each name describing the artifact the skill produces from the previous stage's output.
`tdd` is the exception: it's a technique skill another skill drives, not a stage that hands off its own artifact.

### to-spec

Turns a raw idea or feature request into a written, approved spec through a short interview.
Explores the target project's context first, asks clarifying questions one at a time, and writes the result to `docs/ai-sdlc/specs/`.

### to-ticket

Decomposes an approved spec into tracer-bullet tickets: narrow, independently demoable vertical slices, each sized to fit a single `to-implement` session.
Each ticket declares what blocks it, so the set of ready-to-start work is always visible.

### tdd

Drives a single red-green cycle at a named seam: one failing test, then the minimal code to pass it.
Refactoring is deliberately excluded here; that's `to-code-review`'s job once the code is green.
This one breaks the `to-<noun>` naming pattern on purpose - it's a technique another skill drives, not a stage that hands off its own artifact.

### to-implement

Implements a ticket's described behavior as working code, driving `tdd` at each seam rather than writing tests or implementation directly.
Runs the full test suite once every seam is green, then hands off to `to-code-review`.

### to-code-review

Reviews a ticket's implementation using the `quality-reviewer` agent, scoped to reuse, simplification, efficiency, and clarity only, and applies fixes directly.
Explicitly does not check correctness or whether the implementation satisfies its acceptance criteria - that stays a separate concern.

## Design

See `docs/specs/2026-07-31-ai-sdlc-plugin-design.md` for the full design rationale, including why the repo is structured as a single self-referencing plugin and marketplace, and `docs/plans/2026-07-31-ai-sdlc-plugin-implementation-plan.md` for how it was built.

## License

MIT, see [LICENSE](LICENSE).
