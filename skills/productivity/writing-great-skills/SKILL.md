---
name: writing-great-skills
description: Apply craft principles for writing effective Claude Code skills - predictability over stochastic output, progressive disclosure, tight descriptions, right-sized splitting, and pruning discipline. Use whenever creating a new SKILL.md, editing an existing one, or judging whether a skill's content is worth keeping as-is - triggers on any request to write, draft, tighten, split, or review a skill, agent, or command file, even if the user doesn't say "review" or name this skill directly.
---

# writing-great-skills

## Purpose

A skill exists to wrangle determinism out of a stochastic system.
Predictability, the agent taking the same process every run, not necessarily producing the same output, is the root virtue a skill is judged on.
Everything below is in service of that one property: apply it whenever writing a new `SKILL.md`, editing an existing one, or judging whether a skill's content earns its place.

This skill covers craft principles: what makes the content of a skill good.
It's a companion to `skill-creator`, which covers the process: interviewing intent, drafting, running test cases, and iterating on feedback.
Use `skill-creator` to run that workflow; use this skill as the quality bar you hold the draft against, whether it came out of that workflow or was hand-written.

## Two invocation modes

Every skill picks one, and the choice belongs in its frontmatter (`disable-model-invocation: true` for the second):

- **Model-invoked**: discoverable automatically through its always-loaded description. Costs a small slice of every session's context, whether or not the skill ever fires, so the description has to earn that cost.
- **User-invoked only**: zero background context cost, but only runs when someone remembers to ask for it by name. Right for anything niche enough that auto-discovery would rarely be the actual trigger.

Pick based on how the skill gets found in practice, not on how important it feels.

## Information hierarchy

Skills load in three tiers, from always-present to loaded-on-demand.
Put content at the shallowest tier it can survive at, and use the depth below as the test for whether something belongs deeper:

1. **In-skill steps**: ordered actions with a checkable, exhaustive completion criterion. This is what most of a skill's body should be.
2. **In-skill reference**: flat definitions or rules consulted on demand, not read start to finish every time.
3. **External reference**: pushed to a linked file (`references/*.md`, `scripts/*`) that's only read when a step actually points at it.

If a section branches ("if X, see Y; otherwise see Z"), that branch is the signal it belongs one tier deeper, not that it needs more words where it already sits.

## Writing the description

The description is the only part of a model-invoked skill that's always in context, so it carries the full weight of triggering correctly:

- Front-load the leading word: the concept someone would search or think of first, not a scene-setting clause before it.
- One trigger phrase per distinct branch of when to use the skill. Collapse synonyms into that one phrase rather than listing near-duplicates.
- Strip anything that's just restating the skill's identity ("this skill helps you...") instead of naming a concrete triggering context.
- Write it a little pushy: name the phrases and contexts that should trigger it explicitly, since the failure mode on this axis is under-triggering, not over-triggering.

## Granularity and splitting

Two different reasons justify splitting one skill into two:

- **Split by invocation** when a piece of the content has its own distinct leading word people would reach for independently. If it needs its own trigger, it's arguably its own skill.
- **Split by sequence** when steps after a natural completion point tempt the agent (or the user) to call the skill "done" early. A skill that quietly expects a second phase after its own obvious finish line will get abandoned there; split the phases into two skills with an explicit handoff instead.

## Pruning discipline

Skills accumulate content faster than anyone re-reads them, so pruning has to be an active practice, not a side effect of noticing clutter:

- One meaning, one source of truth. If a rule is stated in two places, one of them will drift; delete the copy and point to the original instead.
- Run the no-op test sentence by sentence: if the agent would already do this by default without the sentence, the sentence is dead weight. Delete failures entirely, don't soften them into a hedge.
- When you prune, delete whole sentences. Trimming a sentence down to a fragment usually just makes it more ambiguous, not shorter in any way that matters.

## Leading words

A leading word is a compact, pretrained concept that anchors both execution ("do the red-green cycle") and invocation ("use tdd").
When editing a skill, hunt for places where a verbose restatement could collapse into one of these instead - it's both shorter and more reliably triggers the right prior knowledge in the model reading it.

## Failure modes checklist

Run a skill against this list before considering it finished:

- **Premature completion**: the agent stops before the real end state. Fix by sharpening the completion criterion, or by splitting by sequence (see above).
- **Duplication**: the same rule lives in two places. Fix by picking one source of truth and deleting the other.
- **Sediment**: stale layers left behind by earlier edits that no longer reflect current behavior. Fix by actively pruning on a schedule, not waiting for it to cause a visible bug.
- **Sprawl**: the skill is too long for what it does. Fix by pushing detail down the information hierarchy and splitting where the granularity rules above call for it.
- **No-op**: an instruction that's already the model's default behavior. Fix by deleting it, or, if it's genuinely being missed in practice, stating it more forcefully rather than padding it.
- **Negation**: a rule stated as a ban ("don't do X") instead of a positive target ("do Y"). Fix by rewriting toward the state you actually want, since a positive target is easier to satisfy and easier to verify than the absence of something.

## Credit

Adapted from [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-great-skills), an existing public skill covering the same ground.
