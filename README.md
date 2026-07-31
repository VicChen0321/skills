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

### to-ticket

### tdd

### to-implement

### to-code-review

## Design

See `docs/specs/2026-07-31-ai-sdlc-plugin-design.md` for the full design rationale, including why the repo is structured as a single self-referencing plugin and marketplace, and `docs/plans/2026-07-31-ai-sdlc-plugin-implementation-plan.md` for how it was built.

## License

MIT, see [LICENSE](LICENSE).
