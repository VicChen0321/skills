# CONTEXT.md format

A context file records the project's domain vocabulary: terms, in a hierarchical structure, organized under subheadings once there are enough of them to need grouping.

**Definitions are tight.** One or two sentences, define what a term *is*, not what it does. Every definition includes an `_Avoid_` line listing synonyms to reject, so terminology doesn't drift back to the vague version.

**Only project-specific terms qualify.** Before adding a term, ask whether it's unique to this domain or just a general programming pattern with a project-specific name attached. General patterns don't belong here.

**File layout:**

- Single-context repo: one `CONTEXT.md` at the project root.
- Multi-context repo: a `CONTEXT-MAP.md` at the root listing each context's location and how it relates to the others, plus one `CONTEXT.md` inside each bounded context (for example `src/ordering/CONTEXT.md`).

**Finding the right file:** check for `CONTEXT-MAP.md` first. If it exists, use it to find the right per-context `CONTEXT.md`; if the current topic could belong to more than one context, ask which one before writing anything. If no `CONTEXT-MAP.md` exists, look for a root `CONTEXT.md`. If neither exists, create the root `CONTEXT.md` lazily, only once there's a real term to record.

**Example entry:**

```md
### Fulfillment window

The time range during which an order can still be picked, packed, and shipped to meet its delivery promise.

_Avoid: shipping window, cutoff time._
```
