# HTML report format

One self-contained HTML file, written to the OS temp directory (resolve `$TMPDIR`, fall back to `/tmp` or `%TEMP%`), named `architecture-review-<timestamp>.html` so repeated runs never collide. Tailwind and Mermaid both load from CDN; nothing else is a dependency, and nothing gets written into the repo.

## Scaffold

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Architecture review - {{repo name}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
      import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
      mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
    </script>
  </head>
  <body class="bg-neutral-50 text-neutral-900">
    <main class="max-w-4xl mx-auto px-6 py-10 space-y-10">
      <header><!-- repo name, date, one-line legend --></header>
      <section id="candidates" class="space-y-8"><!-- one card per candidate --></section>
      <section id="top-recommendation"><!-- single closing card --></section>
    </main>
  </body>
</html>
```

## Header

Repo name, date, and a compact legend explaining the diagram conventions used below (solid box = module, dashed = seam, red = leakage across a seam, heavy border = deep). No introductory paragraph - go straight into candidates.

## Candidate card

One `<article>` per candidate, in this order:

- **Title** - names the deepening, not the files (e.g. "Collapse the order-intake pipeline," not "OrderHandler.ts").
- **Badges** - recommendation strength (`Strong` / `Worth exploring` / `Speculative`) and the dependency category from `codebase-design`'s `DEEPENING.md` (in-process, local-substitutable, ports & adapters, mock).
- **Files** - a short monospaced list.
- **Before/after diagram** - the centerpiece; two columns, side by side. Pick a pattern below.
- **Problem** - one sentence, what hurts today.
- **Solution** - one sentence, what changes.
- **Wins** - a short bullet list, each naming the payoff in `codebase-design` terms ("locality: the bug fix lands in one place," "leverage: one interface, five call sites"), not generic phrasing like "cleaner code."
- **ADR callout**, only when relevant - one line noting which ADR this would revisit and why the friction now outweighs it.

If a diagram needs a paragraph to explain, the diagram is wrong - redraw it instead of adding prose.

## Diagram patterns

Vary these across candidates rather than defaulting to one for everything:

- **Mermaid flowchart or sequence diagram** - the default for call chains and dependency graphs. Use `classDef` to color a leaking edge or node distinctly (for example a red stroke) so the eye lands on it immediately.
- **Hand-built boxes with inline SVG connectors** - reach for this when Mermaid's automatic layout fights the point you're making, especially an "after" state that should read as one solid, heavy-bordered module with everything else faded inside it.
- **Stacked cross-section** - horizontal bands for a call passing through layers: many thin bands before, one thick consolidated band after.
- **Paired rectangles for interface vs. implementation** - shows shallowness directly: before, the interface rectangle is nearly as tall as the implementation rectangle; after, the interface shrinks and the implementation absorbs what it used to expose.

## Style

Editorial rather than dashboard-like: generous whitespace, one accent color, red reserved for leakage and amber for ADR warnings. Keep each diagram to roughly the same height so before/after panels line up without scrolling. Label modules inside diagrams in small caps or uppercase tracking so they read as schematic notation, not as UI chrome.

## Top recommendation

One larger closing card: which candidate to tackle first, one sentence why, and an anchor link back to its full card. Nothing more.

## Vocabulary discipline

Every card uses `codebase-design`'s terms - module, interface, implementation, depth, seam, adapter, leverage, locality - and nothing else. Not "component," "service," "API," "boundary," or "layer" where one of those terms already applies. If a win can't be phrased in glossary terms, it probably isn't a real win yet.
