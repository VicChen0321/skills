# ADR format

ADRs live in `docs/adr/`, named sequentially: `0001-slug.md`, `0002-slug.md`. Create the directory only when the first ADR is actually needed.

**Minimal template**, and often all that's needed:

```md
# {Short title of the decision}

{1-3 sentences: what's the context, what did we decide, and why.}
```

The value of an ADR is in recording *that* a decision was made and *why*, not in the formatting. A single paragraph is a complete ADR.

**Optional sections**, add only when they carry real information:

- **Status** - proposed, accepted, deprecated, or superseded.
- **Considered Options** - rejected alternatives worth remembering, so nobody re-litigates them from scratch.
- **Consequences** - non-obvious downstream effects.

**When an ADR is warranted**, all three must hold:

1. Hard to reverse - real cost to changing course later.
2. Surprising without context - a future reader would question the approach without the rationale.
3. A genuine tradeoff - real alternatives existed, and one was deliberately chosen over the others.

**Good candidates:** architectural patterns, integration approaches between systems, technology choices with lock-in potential, ownership boundaries between components, intentional deviations from the conventional approach, non-code constraints, and deliberate rejections of a popular alternative.
