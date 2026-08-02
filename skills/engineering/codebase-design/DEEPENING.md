# Deepening

How to deepen a shallow module (or cluster of related shallow modules) given what it actually depends on. Assumes the vocabulary in [SKILL.md](SKILL.md): module, interface, seam, adapter.

## Classify the dependency first

What a candidate depends on determines whether it needs a seam at all, and if so, where.

1. **In-process** - pure computation, in-memory data, no I/O crossing a process or network boundary. Always safe to merge and deepen; test straight through the new interface, no adapter required.
2. **Local-substitutable** - a real dependency with a faithful local stand-in already available (an in-memory queue, an embedded database like PGLite/SQLite). Deepen the module, keep the seam internal to the implementation, and run tests against the stand-in rather than the real thing.
3. **Owned but remote** - your own service reached over a network. Put a small interface at the seam, let the deep module hold the logic, and inject the transport as an adapter: an HTTP or queue adapter in production, an in-memory one in tests. The network hop moves to the edge; the logic stays in one place.
4. **External and uncontrolled** - a third-party dependency (a payment processor, an email provider). Same shape as (3): inject it as a port, satisfy it with a mock adapter in tests. You can't change its contract, so don't let its shape leak past the seam.

## Seam discipline

Don't add a seam speculatively. A single adapter is not evidence a seam is needed - it usually means the "seam" is just an extra layer of indirection with nothing varying across it. Wait until a second adapter is genuinely required (typically: one for production, one for tests) before introducing the interface that separates them.

A deepened module is also allowed internal seams of its own - places its own tests reach into, invisible to callers. Keep those private; exposing an internal seam through the public interface just to make testing convenient defeats the point of depth.

## Replace tests, don't stack them

Once tests exist at the deepened module's interface, the old unit tests written against the pieces it absorbed are redundant - delete them rather than keeping both layers. New tests should assert on what the interface promises, not on internal state or call order. A test that has to change every time the implementation is refactored, with no change to observable behavior, was never testing the interface in the first place.
