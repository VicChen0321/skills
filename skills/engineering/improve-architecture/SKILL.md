---
name: improve-architecture
description: Scan a codebase for deepening opportunities, present them as a visual HTML report, then work through whichever one gets picked. Use when the user wants an architecture review, asks to find refactor candidates, or says the codebase feels tangled and wants a structured look before diving in.
disable-model-invocation: true
---

# improve-architecture

Find modules worth deepening - where a shallow interface is hiding real complexity, or where complexity is scattered across callers instead of concentrated in one place - and work through the strongest candidate with the user. Uses `codebase-design`'s vocabulary throughout: module, interface, depth, seam, adapter, leverage, locality.

## Process

1. **Scope it.** If the user named a target - a module, a subsystem, a specific pain point - start there. Otherwise, scan `git log --oneline` over a meaningful stretch of history for hot spots: files and areas that keep changing are where a deepening pays off soonest. If nothing stands out, widen the scan.
2. **Read context first.** Load `CONTEXT.md` and any ADRs under the scoped area before forming opinions - domain vocabulary and prior decisions shape what counts as a real problem versus a already-settled tradeoff.
3. **Explore.** If the harness supports subagents, spawn one with `subagent_type=Explore` to walk the scoped code; otherwise explore it directly. Look for: modules whose interface is nearly as complex as what's behind it, understanding that requires bouncing across many small pieces, tests that mock everything because nothing has real depth, and logic that leaks across a seam it's supposed to stay behind. Run the deletion test on every suspect: delete it mentally - does its complexity vanish, or reappear at every caller?
4. **Write the report.** Produce a self-contained HTML file in the OS temp directory (never in the repo) - see [HTML-REPORT.md](HTML-REPORT.md) for the scaffold, card layout, and diagram patterns. Each candidate gets its own card: files touched, the problem, the proposed change, the payoff in terms of leverage/locality, a before/after diagram, and a recommendation strength (`Strong`, `Worth exploring`, `Speculative`). Close with a top recommendation. Open the file for the user (`open`/`xdg-open`/`start` per OS) and state its path. Don't propose concrete interfaces yet - ask which candidate to explore.
5. **Work the chosen candidate.** Invoke `interrogate` to walk through it with the user: constraints, what it depends on, what shape the deepened module takes, what survives behind the seam. If the user wants to compare interface shapes before settling, use `codebase-design`'s design-it-twice process.
6. **Keep the record current as decisions land**, per `domain-modeling`: a deepened module named after a concept missing from `CONTEXT.md` gets added there; the user rejecting a candidate for a load-bearing reason gets offered an ADR, so the same refactor doesn't get re-suggested next time - skip the offer for reasons that are ephemeral or self-evident.
7. **Flag ADR conflicts, don't override them.** If a candidate would contradict a recorded ADR, only surface it when the friction is concrete enough to justify reopening that decision - not every theoretical refactor an ADR rules out.

## Handoff

None automatic. If the interview lands on real work, the user can hand the decided direction to `to-ticket` or `to-implement` themselves.
