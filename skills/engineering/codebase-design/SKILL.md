---
name: codebase-design
description: Shared vocabulary for designing deep modules - module, interface, depth, seam, adapter, leverage, locality. Use when designing or restructuring a module's interface, deciding where a seam belongs, or when another skill needs this vocabulary.
---

# codebase-design

A **module** is anything with an interface and an implementation - a function, a class, a package, or a slice spanning several of those.
Design toward **depth**: a lot of behavior reachable through a small interface.
A module is **deep** when its interface is much simpler than what sits behind it, **shallow** when the interface is nearly as complicated as the implementation it wraps - at that point it's just a pass-through.

## Glossary

Use these terms exactly. Don't drift into "component," "service," "API," or "boundary" - consistent language is the point.

- **Interface** - everything a caller has to know to use the module correctly: not just the type signature, but invariants, ordering constraints, error modes, and required configuration. Broader than "API" or "signature," which only cover the type-level surface.
- **Implementation** - the code behind the interface. A module can be a thin adapter around a large implementation (a Postgres-backed repository) or a large adapter around a small one (an in-memory fake) - reach for "adapter" when the seam itself is the topic, "implementation" otherwise.
- **Depth** - how much behavior a caller or test reaches per unit of interface they have to learn. High depth means high leverage per line of interface.
- **Seam** (Michael Feathers) - the place where a module's interface lives, where behavior can be substituted without editing the code at that spot. Not the same as a DDD bounded context - say "seam" here, not "boundary."
- **Adapter** - whatever satisfies an interface at a seam. Names a role (what slot it fills), not what's inside it.
- **Leverage** - what a caller gets from depth: one implementation, reused correctly across every call site that goes through the same small interface.
- **Locality** - what a maintainer gets from depth: a bug, a behavior change, or a piece of domain knowledge lives in one module instead of being smeared across every caller.

## Deep vs. shallow

```
Deep:                          Shallow:
┌───────────────┐              ┌───────────────────────────┐
│ small interface│              │      large interface       │
├───────────────┤              ├───────────────────────────┤
│               │              │   thin pass-through body   │
│  large body   │              └───────────────────────────┘
│               │
└───────────────┘
```

When shaping an interface, keep asking: can this lose a method, a parameter, a config flag, and still cover the same ground?

## Principles

- **The deletion test.** Imagine deleting the module. If its complexity disappears with it, it was a pass-through earning nothing. If that complexity reappears at every caller instead, the module was doing real work.
- **The interface is the test surface.** A test that reaches past the interface to assert on internals is testing the wrong thing - it should break only when the interface's contract changes, not when the implementation is reshuffled.
- **One adapter is a hypothetical seam; two adapters make it real.** Don't introduce a seam for a variation that doesn't actually exist yet - a mock alone doesn't justify one, a second production-grade adapter does.
- **Depth lives at the interface, not inside it.** A deep module can be built from many small internal pieces; those pieces aren't part of what callers see, and don't need their own seams unless the module's own tests want them.

## Designing for testability

Depth and testability come from the same habits: accept collaborators as arguments instead of constructing them inside, return computed results instead of mutating shared state, and keep the interface small enough that a test's setup stays short. A module that needs a paragraph of mocking to test is usually shallow, not under-tested.

## Going deeper

- Deepening a module given what it depends on - see [DEEPENING.md](DEEPENING.md).
- Exploring more than one interface shape before committing - see [DESIGN-IT-TWICE.md](DESIGN-IT-TWICE.md).
