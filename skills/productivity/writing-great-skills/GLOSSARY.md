# Glossary

## Invocation

**Predictability**
A skill is judged by whether it follows the same process every time it runs, not by whether it produces the same output.

**Model-invoked**
A skill discoverable through its own description without anyone typing its name - the agent, or another skill, can find and fire it unprompted.

**User-invoked**
A skill reachable only by typing its name directly (`disable-model-invocation: true`). It never fires on its own.

**Description**
The frontmatter summary. For a model-invoked skill it has to both identify the skill and list every situation that should trigger it. For a user-invoked skill it only needs to explain what the skill is for.

**Context load**
The ongoing cost of a model-invoked skill: its description occupies space in every session regardless of whether the skill is actually used that session.

**Cognitive load**
The cost that replaces context load for a user-invoked skill: someone has to remember the skill exists and call it by name.

**Context pointer**
A sentence inside a skill whose wording determines whether the agent goes and reads a linked file. The value is entirely in how it's phrased, not in where the file sits.

**Router**
A single user-invoked skill whose only job is naming a set of other skills and saying when each one applies, used once there are too many of them to remember unassisted.

**Granularity**
The choice of how many separate skills a set of related behavior gets split into. Every additional skill costs either context load or cognitive load.

## Information hierarchy

**Step**
An action written directly into a skill's main flow, meant to be carried out in order.

**Completion criterion**
The test that tells the agent a step is actually finished, not just attempted. Should be checkable, and thorough enough to catch partial completion where that matters.

**Legwork**
The actual investigation or effort the agent puts in to satisfy a completion criterion.

**Reference**
A fact, rule, or definition written into the skill for lookup, rather than an action to perform in sequence.

**External reference**
Reference material moved out of the main skill file into a separate one, only read when a context pointer sends the agent there.

**Progressive disclosure**
Deliberately keeping a skill's visible body short by pushing detail into files that only load when actually needed.

**Co-location**
Keeping everything related to one idea, its definition, its exceptions, its caveats, together in one place rather than spread across the skill.

**Branch**
One of several distinct situations a single skill is meant to handle, each potentially triggering a different path through it.

**Sprawl**
A skill that has grown too long, even without containing anything actually wrong or extraneous.

## Steering

**Leading word**
A single word or short phrase the model already strongly associates with a specific behavior, letting it stand in for a much longer explanation.

**Post-completion steps**
Steps that come after the one currently in progress, visible enough to pull the agent's attention forward and tempt it to rush the current one.

**Premature completion**
Treating a step as done before it actually meets its completion criterion.

**Negation**
Instructing the agent by naming what not to do, which tends to draw more attention to the banned behavior rather than less.

## Pruning

**Single source of truth**
The practice of stating each fact in exactly one place in a skill, so it never needs to be updated in more than one spot.

**Duplication**
The same fact appearing in more than one place, which both risks the copies drifting apart and overstates how much space that fact deserves.

**Relevance**
Whether a given line still describes something the skill actually needs to do, independent of whether it's duplicated anywhere.

**Sediment**
Old material that stays in a skill because removing it feels riskier than leaving it, even after it stops being useful.

**No-op**
An instruction that doesn't change what the agent does, because the agent was already going to behave that way regardless.
