# Design it twice

For exploring more than one interface shape for a chosen deepening candidate before committing to one. The premise (Ousterhout's "Design It Twice"): a first interface is rarely the best one, and the fastest way to find a better one is to force a genuinely different alternative into existence, not to polish the first draft.
Assumes the vocabulary in [SKILL.md](SKILL.md): module, interface, seam, adapter, leverage.

## Process

1. **Frame the constraints.** Before generating alternatives, write down what any interface for this candidate would have to satisfy: the dependency category it falls into (see [DEEPENING.md](DEEPENING.md)), what callers currently need from it, and a short illustrative sketch - not a proposal, just enough code to make the constraints concrete. Share this with the user.
2. **Produce genuinely different alternatives.** If the harness supports subagents, spawn three or more in parallel, each briefed with the same constraints from step 1 plus one distinct design pressure to push against:
   - minimize the interface to as few entry points as possible, maximizing leverage per entry point;
   - maximize flexibility for callers with needs not yet known;
   - optimize for the single most common caller, making that path trivial even if others get more awkward;
   - lean on ports and adapters if the candidate's dependency crosses a real seam (category 3 or 4 in [DEEPENING.md](DEEPENING.md)).
   Give each brief both this file's vocabulary and the project's own `CONTEXT.md` terms, so every alternative names things the same way. Without subagent support, produce the alternatives yourself in sequence, holding one design pressure at a time so they don't blur into variations of the same idea.
3. **Have each alternative report:** the interface itself (types or signatures, invariants, error modes), a usage example, what it hides behind the seam, its adapter strategy, and where its leverage is strong versus thin.
4. **Compare, then recommend.** Lay out the alternatives side by side against depth, locality, and where the seam ends up. Say which one you'd pick and why, or propose a hybrid if two alternatives each got something right. Give a clear read, not an even-handed menu.
