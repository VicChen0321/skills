---
name: domain-modeling
description: Build and sharpen a project's domain model - ubiquitous language, architectural decisions, and domain vocabulary. Use when pinning down terminology, or when another skill needs the domain model maintained.
---

# domain-modeling

This is active work: challenge terminology, create edge-case scenarios, and document glossary entries and decisions as they emerge.
Passively reading existing context files doesn't count.

**File organization.** Single-context repos: `CONTEXT.md` at the root, `docs/adr/` for decisions. Multi-context repos: a root `CONTEXT-MAP.md` pointing to a `CONTEXT.md` inside each bounded context (for example `src/ordering/CONTEXT.md`). Create files on demand, only once there's real content to record.

**During a session:**

- Challenge the existing glossary: flag it immediately when what the user says conflicts with documented terminology.
- Sharpen vague terms: when vocabulary is overloaded or unclear, propose a precise canonical term.
- Stress-test with scenarios: probe relationships and force precision about concept boundaries with concrete edge cases.
- Cross-reference code: verify stated behavior against the actual implementation, and surface contradictions.
- Update inline: capture a resolved term in `CONTEXT.md` the moment it's resolved, never batched at the end.
- Write an ADR only when all three hold: the decision is hard to reverse, it would be surprising without the rationale recorded, and it represents a genuine tradeoff.
