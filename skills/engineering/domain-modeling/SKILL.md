---
name: domain-modeling
description: Build and sharpen a project's domain model - ubiquitous language, architectural decisions, and domain vocabulary. Use when pinning down terminology, or when another skill needs the domain model maintained.
---

# domain-modeling

This is active work: challenge terminology, create edge-case scenarios, and document glossary entries and decisions as they emerge.
Passively reading existing context files doesn't count.

**File structure.** Most repos have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-choose-event-bus.md
│       └── 0002-adopt-postgres.md
└── src/
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts.
The map points to where each one lives:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                    <- system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/           <- context-specific decisions
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Create files lazily - only when there's something to write.

**During a session:**

- Challenge the existing glossary: flag it immediately when what the user says conflicts with documented terminology.
- Sharpen vague terms: when vocabulary is overloaded or unclear, propose a precise canonical term.
- Stress-test with scenarios: probe relationships and force precision about concept boundaries with concrete edge cases.
- Cross-reference code: verify stated behavior against the actual implementation, and surface contradictions.
- Update inline: capture a resolved term or decision the moment it's resolved, never batched at the end.

**Where things go:**

- Domain vocabulary: `CONTEXT.md`. Format and file-finding rules in `CONTEXT-FORMAT.md`.
- Architecture decisions: `docs/adr/` - root for system-wide, per-context for context-specific. Format and when-to-write-one rules in `ADR-FORMAT.md`.

Read the relevant format file before writing to either location for the first time in a session.
