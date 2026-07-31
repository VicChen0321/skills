---
name: writing-great-skills
description: Vocabulary and craft principles for writing and reviewing Claude Code skills. Reference for tightening a SKILL.md, choosing invocation mode, or deciding what to split or prune.
disable-model-invocation: true
---

# writing-great-skills

The measure of a good skill isn't the output it produces on any single run, it's whether it produces that output the same way every time.
Call that **predictability**.
Everything below exists to protect it.

Terms in **bold** are defined once in [`GLOSSARY.md`](GLOSSARY.md); this file uses them without re-explaining them each time.

## Choosing how a skill gets found

A skill can be found two ways, and each has a cost attached.

Make it **model-invoked** and it carries a live description the agent scans every session, whether or not the skill ever runs - that's **context load**, paid continuously in exchange for the agent (or another skill) being able to reach it without anyone asking by name.

Make it **user-invoked** instead (`disable-model-invocation: true`) and that per-session cost disappears, but a new one appears: **cognitive load**, the burden of a person having to remember the skill exists at all and call it by name when needed.

Default to user-invoked. Reserve model-invoked for cases where nothing else can reach the skill without it announcing itself. Once enough user-invoked skills pile up that nobody can hold the full list in their head, build a **router**: a single user-invoked skill whose whole job is pointing at the others.

## Writing a description that pulls its weight

For a model-invoked skill, the description carries two jobs at once: saying what the skill does, and naming every distinct **branch**, every different situation, that should make it fire.

Three habits keep it earning its cost:

- Put the word that most strongly identifies the skill first, not buried after a preamble.
- Give each branch exactly one line. Two phrasings of the same trigger are **duplication** wearing a disguise; merge them.
- Leave out anything the body already says about what the skill is - the description's job is triggering, not re-introducing.

A user-invoked skill's description skips all of this; it only has to tell a person what the skill is for.

## Deciding what goes where

Everything in a skill is either something the agent *does* (a **step**) or something it *looks up* (**reference**), and either can sit at one of three depths depending on how urgently the agent needs it:

1. Written directly into the flow of `SKILL.md` as an ordered action - where most of a skill's real work should live. Every such step needs a **completion criterion** that's checkable and, wherever it matters, thorough enough to rule out partial credit; a loose one invites **premature completion**.
2. Written directly into `SKILL.md`, but consulted only when needed rather than read top to bottom. A flat list of rules is a perfectly normal shape here, not a warning sign.
3. Moved out of `SKILL.md` entirely into a sibling file, reachable only through a **context pointer** - a sentence whose exact wording, not the file's location, decides whether the agent bothers to go read it.

Moving something from the first depth toward the third is **progressive disclosure**: it keeps the visible surface of the skill short by trusting the agent to fetch detail only once a pointer actually sends it there.
Wherever something lands, keep its full story together, definition, exceptions, caveats, under one heading instead of scattering pieces of it across the file. That's **co-location**.

## When one skill should become two

Splitting costs one of the two loads above every time, so only do it when the split earns that cost back. Two situations justify it:

- A chunk of content has a name distinctive enough that people would reach for it on its own - split it into its own model-invoked skill rather than burying a second identity inside the first.
- A long run of steps has later steps visible to the agent while it's still working an earlier one, and those **post-completion steps** pull its attention forward, tempting it to wrap up the current step early. Hide them behind a split so there's nothing ahead to rush toward.

## Keeping it lean

Every fact in a skill should live in exactly one place, a **single source of truth**, so changing how the skill behaves never means hunting down a second copy to update too.

Periodically ask, line by line, whether each one still describes something the skill actually does; a line that's stopped being true or stopped mattering is dead weight regardless of how it got there.

Look for **no-ops**: instructions describing what the agent would have done anyway. Test each sentence in isolation - if removing it wouldn't change a single run's behavior, remove it for good rather than softening it.

## Borrowing words the model already knows

Some words carry an entire behavior inside a single token, because the model already has a strong, consistent association with them from training. Call these **leading words**.

Used in a skill's body, one anchors an entire block of behavior every time it appears. Used in a description, it gives the agent something concrete to match against instead of a vague paraphrase.

Look for places where a skill spends several sentences on something a single well-chosen word would carry alone: for example, replacing a paragraph about avoiding wasted motion and unnecessary steps with the single word *lean*, or replacing "the point where every test in the suite passes" with just *green*. One good word usually beats a full sentence, both in length and in how reliably the model responds to it.

## Common ways a skill goes wrong

- **Premature completion**: the agent calls something finished before it actually is. First response: make the completion criterion harder to satisfy loosely. Only reach for a split if the criterion genuinely can't be sharpened further and the early-stopping still happens in practice.
- **Duplication**: one fact stated in two places. Beyond the maintenance cost, it also makes that fact seem more important than it actually is, since it appears to occupy more of the skill than it really does.
- **Sediment**: old instructions nobody removes, because adding new ones feels safer than deleting old ones. Left unchecked, every skill accumulates this over time.
- **Sprawl**: a skill that's long even though nothing in it is wrong or unnecessary. The fix isn't cutting content, it's moving it: push detail to a deeper tier, or split along one of the two lines above.
- **No-op**: an instruction that changes nothing because the agent was already going to do it. If the intent behind it is real but being ignored in practice, the fix is usually a stronger word, not a longer sentence.
- **Negation**: telling the agent what not to do. Saying the banned thing out loud tends to make it more present in the agent's attention, not less. State what you actually want instead, and keep an explicit "don't" only for the rare case with no positive phrasing at all.
