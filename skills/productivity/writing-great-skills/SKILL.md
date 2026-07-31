---
name: writing-great-skills
description: Craft principles for writing effective Claude Code skills - predictability, progressive disclosure, tight descriptions, right-sized splitting, pruning. Use when creating or editing a SKILL.md, or reviewing one for quality.
---

# writing-great-skills

**Predictability is the root virtue.** A skill exists to wrangle determinism out of a stochastic system: the agent takes the same process every run, not necessarily the same output.

**Two invocation modes.** Model-invoked: discoverable via its always-loaded description, costs context every session. User-invoked only (`disable-model-invocation: true`): zero background cost, only runs when asked for by name.

**Information hierarchy**, shallowest to deepest:

1. In-skill steps - ordered actions, checkable completion criteria.
2. In-skill reference - flat rules, consulted on demand.
3. External reference - linked files, read only when a step points at them.

A branching section ("if X see Y, else see Z") belongs one tier deeper.

**Descriptions**: front-load the leading word. One trigger phrase per branch, collapse synonyms. Strip anything that just restates identity. Write it a little pushy - under-triggering is the real risk.

**Splitting**: split by invocation when a part has its own distinct leading word. Split by sequence when steps after a natural stopping point tempt early completion.

**Pruning**: one meaning, one source of truth. Run the no-op test sentence by sentence - if the agent would already do it by default, delete the sentence, don't soften it.

**Leading words**: compact, pretrained concepts that anchor execution and invocation. Collapse verbose restatements into one where you can.

**Failure modes:**

- Premature completion - sharpen the criterion, or split by sequence.
- Duplication - one source of truth.
- Sediment - stale layers, prune actively.
- Sprawl - too long, push detail down the hierarchy or split.
- No-op - already default behavior, delete or state more forcefully.
- Negation - "don't do X" instead of "do Y."
