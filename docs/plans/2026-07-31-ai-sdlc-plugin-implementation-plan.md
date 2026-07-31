# AI-SDLC Skills Plugin Implementation Plan

> **Spec:** `docs/specs/2026-07-31-ai-sdlc-plugin-design.md`. Every task below implements one section of it; the plan doesn't re-derive design decisions, it follows the spec.

**Goal:** Build the `ai-sdlc` Claude Code plugin: five skills (`to-spec`, `to-ticket`, `tdd`, `to-implement`, `to-code-review`) plus the `quality-reviewer` agent, installable through the plugin marketplace flow.

**Architecture:** Single-plugin repo that is also its own marketplace root, the same pattern the installed `superpowers` plugin uses. Skills live under `skills/engineering/`, discovered via an explicit entry in `plugin.json`'s `skills` array. One plugin-owned agent at `agents/quality-reviewer.md`.

**Tech Stack:** Markdown files with YAML frontmatter (`SKILL.md`, the agent file) and JSON manifests (`plugin.json`, `marketplace.json`). This repo contains no application code; its "tests" are frontmatter/JSON validity checks plus a live end-to-end run against a throwaway sample project.

## Global Constraints

- No em dash ("-" only, never "—") anywhere in any file this plan creates.
- Long-form prose in any markdown file: one full sentence per line. Normal Markdown structure (headers, lists, code fences) is unaffected by this rule.
- License: MIT, copyright holder Vic Chen.
- No `CHANGELOG.md`.
- Plugin name `ai-sdlc`, starting version `0.1.0`.
- All five skills live under `skills/engineering/`.
- Only commit when explicitly told to during execution; the commit steps below describe the intended history, they are not standing authorization to run `git commit` unattended.

---

## Task 1: Plugin and marketplace manifests

**Files:**
- Create: `.claude-plugin/plugin.json`
- Create: `.claude-plugin/marketplace.json`

**Interfaces:**
- Produces: the `ai-sdlc` plugin identity and the `"skills": ["./skills/engineering/"]` scan declaration every skill file in Tasks 3-7 depends on to be discovered at all.

- [ ] **Step 1: Write plugin.json**

```json
{
  "name": "ai-sdlc",
  "description": "AI-assisted SDLC skills for Claude Code: spec, ticket, TDD, implement, and code review, with test/ship stages to come",
  "version": "0.1.0",
  "author": { "name": "Vic Chen", "email": "victor72441299@gmail.com" },
  "homepage": "https://github.com/<your-github-username>/skills",
  "repository": "https://github.com/<your-github-username>/skills",
  "license": "MIT",
  "keywords": ["skills", "sdlc", "spec", "ticket", "tdd", "code-review", "planning", "workflow"],
  "skills": ["./skills/engineering/"]
}
```

Leave the `<your-github-username>` placeholders as-is; they get filled in once this repo has a GitHub remote, outside the scope of this plan.

- [ ] **Step 2: Write marketplace.json**

```json
{
  "name": "ai-sdlc-marketplace",
  "description": "Marketplace for Vic's AI-SDLC skills plugin",
  "owner": { "name": "Vic Chen", "email": "victor72441299@gmail.com" },
  "plugins": [
    {
      "name": "ai-sdlc",
      "description": "AI-assisted SDLC skills for Claude Code",
      "version": "0.1.0",
      "source": "./",
      "author": { "name": "Vic Chen", "email": "victor72441299@gmail.com" }
    }
  ]
}
```

- [ ] **Step 3: Validate JSON syntax**

Run: `python3 -m json.tool .claude-plugin/plugin.json && python3 -m json.tool .claude-plugin/marketplace.json`
Expected: both print reformatted JSON, no parse errors.

- [ ] **Step 4: Commit**

```bash
git add .claude-plugin/plugin.json .claude-plugin/marketplace.json
git commit -m "Add ai-sdlc plugin and marketplace manifests"
```

---

## Task 2: README skeleton and LICENSE

**Files:**
- Create: `LICENSE`
- Create: `README.md`

**Interfaces:**
- Consumes: plugin name and description from Task 1.
- Produces: the repo's public entry point. Per the spec's Goals, a stranger should be able to clone this repo, install it, and understand every skill from this file alone.

- [ ] **Step 1: Write LICENSE**

Standard MIT license text, copyright holder "Vic Chen", year 2026.

- [ ] **Step 2: Write README.md skeleton**

Sections:
- Title and one-line description of the plugin.
- Install: the `/plugin marketplace add <you>/skills` then `/plugin install ai-sdlc` flow from the spec's Install Flow section, plus the local-dev alternative, `claude --plugin-dir .`, for testing before the repo has a remote.
- Skills table: the spec's Roadmap table, verbatim (skill name, input -> output, status).
- A "Skills" section with one subheading per skill, left as just the headings for now (`### to-spec`, `### to-ticket`, `### tdd`, `### to-implement`, `### to-code-review`) - Task 8 fills these in once the skill files exist, so their descriptions can be pulled from the real frontmatter instead of duplicated by hand now.
- License section linking to `LICENSE`.

- [ ] **Step 3: Commit**

```bash
git add LICENSE README.md
git commit -m "Add README skeleton and LICENSE"
```

---

## Task 3: to-spec skill

**Files:**
- Create: `skills/engineering/to-spec/SKILL.md`

**Interfaces:**
- Produces: the `to-spec` skill, frontmatter `name: to-spec`. First skill in the pipeline; consumes nothing from other tasks.

- [ ] **Step 1: Write SKILL.md**

Frontmatter:

```yaml
---
name: to-spec
description: Turn a raw idea or feature request into a written, approved spec through a short interview. Use when the user describes a new feature, change, or idea and hasn't written down what "done" looks like - phrases like "let's build X," "I want to add Y," "spec out Z."
---
```

Body: implement the Process (7 steps), the spec document template, and the Handoff, exactly as written in the spec's "Skill: to-spec" section.

- [ ] **Step 2: Validate frontmatter**

Run: `python3 -c "import yaml; yaml.safe_load(open('skills/engineering/to-spec/SKILL.md').read().split('---')[1])"`
Expected: no exception; confirms valid YAML with `name` and `description` present.

- [ ] **Step 3: Commit**

```bash
git add skills/engineering/to-spec/SKILL.md
git commit -m "Add to-spec skill"
```

---

## Task 4: to-ticket skill

**Files:**
- Create: `skills/engineering/to-ticket/SKILL.md`

**Interfaces:**
- Consumes: spec files written by `to-spec` (Task 3) at `docs/ai-sdlc/specs/*.md` in a consuming project. This is a file-path convention, not a code dependency.
- Produces: the `to-ticket` skill, frontmatter `name: to-ticket`.

- [ ] **Step 1: Write SKILL.md**

Frontmatter:

```yaml
---
name: to-ticket
description: Decompose an approved spec into tracer-bullet tickets - narrow, independently demoable vertical slices with declared blocking order. Use once a spec exists and the user wants it broken into actionable work items, or says "break this into tickets," "what's the backlog for this."
---
```

Body: implement the Process (6 steps), the tracer-bullet framing, the ticket file template, and the Handoff, exactly as written in the spec's "Skill: to-ticket" section.

- [ ] **Step 2: Validate frontmatter**

Run: `python3 -c "import yaml; yaml.safe_load(open('skills/engineering/to-ticket/SKILL.md').read().split('---')[1])"`
Expected: no exception.

- [ ] **Step 3: Commit**

```bash
git add skills/engineering/to-ticket/SKILL.md
git commit -m "Add to-ticket skill"
```

---

## Task 5: tdd skill

**Files:**
- Create: `skills/engineering/tdd/SKILL.md`

**Interfaces:**
- Produces: the `tdd` skill, frontmatter `name: tdd`. A leaf technique skill: callable standalone, and invoked by `to-implement` (Task 6) once per seam.
- Consumes: nothing from other tasks.

- [ ] **Step 1: Write SKILL.md**

Frontmatter:

```yaml
---
name: tdd
description: Drive one red-green cycle at a single named seam - one failing test, then the minimal code to pass it, no refactoring mixed in. Use for any test-first implementation work, standalone or when to-implement invokes it per seam.
---
```

Body: implement the Process (6 steps), the Anti-patterns to reject list, and the Handoff, exactly as written in the spec's "Skill: tdd" section.

- [ ] **Step 2: Validate frontmatter**

Run: `python3 -c "import yaml; yaml.safe_load(open('skills/engineering/tdd/SKILL.md').read().split('---')[1])"`
Expected: no exception.

- [ ] **Step 3: Commit**

```bash
git add skills/engineering/tdd/SKILL.md
git commit -m "Add tdd skill"
```

---

## Task 6: to-implement skill

**Files:**
- Create: `skills/engineering/to-implement/SKILL.md`

**Interfaces:**
- Consumes: ticket files from `to-ticket` (Task 4) at `docs/ai-sdlc/tickets/<spec-slug>/NN-*.md`; invokes the `tdd` skill (Task 5) by its frontmatter `name`, once per seam.
- Produces: the `to-implement` skill, frontmatter `name: to-implement`.

- [ ] **Step 1: Write SKILL.md**

Frontmatter:

```yaml
---
name: to-implement
description: Implement a ticket's described behavior as working code, driving the tdd skill at each seam, then handing off to to-code-review. Use when the user points at a ticket file or says "implement ticket NN," "let's build this ticket."
---
```

Body: implement the Process (7 steps) exactly as written in the spec's "Skill: to-implement" section. Step 4 must explicitly instruct invoking the `tdd` skill per seam, and explicitly forbid writing implementation code outside a `tdd` cycle. End with the Handoff to `to-code-review`.

- [ ] **Step 2: Validate frontmatter**

Run: `python3 -c "import yaml; yaml.safe_load(open('skills/engineering/to-implement/SKILL.md').read().split('---')[1])"`
Expected: no exception.

- [ ] **Step 3: Commit**

```bash
git add skills/engineering/to-implement/SKILL.md
git commit -m "Add to-implement skill"
```

---

## Task 7: to-code-review skill and quality-reviewer agent

**Files:**
- Create: `agents/quality-reviewer.md`
- Create: `skills/engineering/to-code-review/SKILL.md`

**Interfaces:**
- Consumes: the diff left by `to-implement` (Task 6). The skill invokes the agent by its frontmatter `name: quality-reviewer`.
- Produces: the last two components of this iteration's pipeline.

One task because the skill and agent are two halves of one contract: the skill's step "spawn the `quality-reviewer` agent" only makes sense reviewed against the agent's own scope boundary, and vice versa.

- [ ] **Step 1: Write agents/quality-reviewer.md**

Frontmatter:

```yaml
---
name: quality-reviewer
description: Reviews a code diff for reuse, simplification, efficiency, and clarity only, not correctness. Invoked by to-code-review after to-implement finishes a ticket.
model: sonnet
tools: Read, Grep, Glob, Edit, Bash
---
```

Body: a system prompt implementing the "Scope" and "Explicitly out of scope" lists from the spec's "Agent: quality-reviewer" section verbatim - the four in-scope categories (reuse, simplification, efficiency, clarity including the full Fowler smell catalog) and the explicit exclusion of correctness, logic errors, and acceptance-criteria compliance.

- [ ] **Step 2: Write skills/engineering/to-code-review/SKILL.md**

Frontmatter:

```yaml
---
name: to-code-review
description: Review a ticket's implementation for reuse, simplification, efficiency, and clarity using the quality-reviewer agent, then apply fixes directly. Use when a ticket's status is In Review, or the user asks to clean up or review recent changes. Does not check correctness - that stays a separate concern.
---
```

Body: implement the Process (5 steps) exactly as written in the spec's "Skill: to-code-review" section: determine the diff, spawn `quality-reviewer` via the Agent tool, let it apply fixes directly, re-run the full test suite, revert any fix that breaks a previously-passing test, update the ticket status to `Done`.

- [ ] **Step 3: Validate frontmatter for both files**

Run:
```bash
python3 -c "import yaml; yaml.safe_load(open('agents/quality-reviewer.md').read().split('---')[1])"
python3 -c "import yaml; yaml.safe_load(open('skills/engineering/to-code-review/SKILL.md').read().split('---')[1])"
```
Expected: no exceptions.

- [ ] **Step 4: Commit**

```bash
git add agents/quality-reviewer.md skills/engineering/to-code-review/SKILL.md
git commit -m "Add to-code-review skill and quality-reviewer agent"
```

---

## Task 8: Fill in README's per-skill sections

**Files:**
- Modify: `README.md` (the `### to-spec` etc. headings left empty by Task 2, Step 2)

**Interfaces:**
- Consumes: the `description` frontmatter from all five `SKILL.md` files (Tasks 3-7) and from `agents/quality-reviewer.md` (Task 7).

- [ ] **Step 1: Write each skill's section**

For each of the five skills, in pipeline order (to-spec, to-ticket, tdd, to-implement, to-code-review), write 2-3 sentences under its heading, drawn from that skill's own `Purpose`/`description`. Mention `quality-reviewer` under the `to-code-review` entry, since it's the mechanism, not a separately-installed skill.

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "Fill in per-skill descriptions in README"
```

---

## Task 9: Local install verification

**Files:** none created. This task exercises Tasks 1-8's output in a real Claude Code session.

- [ ] **Step 1: Validate the plugin manifest**

Run: `claude plugin validate . --strict`
Expected: no errors. Any warning about an unrecognized field points at a typo against the schema documented in the spec's Repo & Plugin Structure section.

- [ ] **Step 2: Load the plugin locally without a marketplace**

Run: `claude --plugin-dir . plugin list`
Expected: `ai-sdlc` appears, version `0.1.0`.

- [ ] **Step 3: Confirm all five skills and the agent are discovered**

Inside a `claude --plugin-dir .` session, open `/plugin` and inspect the `ai-sdlc` detail view.
Expected: `to-spec`, `to-ticket`, `tdd`, `to-implement`, `to-code-review`, and agent `quality-reviewer` are all listed. If any skill under `skills/engineering/` is missing, the bug is almost certainly in `plugin.json`'s `skills` array from Task 1 - re-check it before touching anything else.

- [ ] **Step 4: No commit unless a bug is found**

This task only verifies. If Step 1 or Step 3 surfaces a real bug, fix it in the relevant task's files and create a new commit describing the fix; don't amend an earlier commit.

---

## Task 10: End-to-end pipeline dry run

**Files:** none in this repo. Produces throwaway files in a sample project outside this repo.

- [ ] **Step 1: Create a throwaway sample project**

Run: `mkdir -p /private/tmp/claude-501/-Users-vicchen-Repositories-skills/*/scratchpad/ai-sdlc-sample && cd <that dir> && git init -q`, or any scratch directory outside version control that matters. No real application code is needed yet; this task exercises the skills' interview and file-writing behavior, not a real feature.

- [ ] **Step 2: Run to-spec on a small fake feature**

Inside a `claude --plugin-dir <path-to-this-repo>` session started in the sample project, describe a small fake feature (for example "add a `--dry-run` flag to a hypothetical CLI tool") and go through the `to-spec` interview.
Expected: a spec file appears at `docs/ai-sdlc/specs/YYYY-MM-DD-dry-run-flag-spec.md` in the sample project, following the template from the spec, and the skill asked clarifying questions rather than guessing.

- [ ] **Step 3: Run to-ticket on the resulting spec**

Expected: ticket files appear at `docs/ai-sdlc/tickets/dry-run-flag/NN-*.md` plus a `README.md` index, each ticket a vertical slice with a `Blocked by` field.

- [ ] **Step 4: Run to-implement on the first unblocked ticket**

Expected: the transcript shows `to-implement` explicitly invoking the `tdd` skill as a distinct step per seam, not inlining test-writing and implementation itself.

- [ ] **Step 5: Run to-code-review**

Expected: the `quality-reviewer` agent runs, applies at least one plausible cleanup if the sample code has any (naming, duplication), and the transcript shows a full test-suite re-run after the fix.

- [ ] **Step 6: Confirm the scope boundary**

Seed one deliberate correctness bug (for example an off-by-one) and one deliberate style issue (for example a bad variable name) into the sample project, then run `to-code-review` again.
Expected: the style issue gets fixed; the correctness bug is left untouched and unmentioned. This is the spec's core scope boundary between `to-code-review` and any future correctness-focused review skill - if this fails, the `quality-reviewer` agent's "Explicitly out of scope" instructions from Task 7 need to be stronger.

- [ ] **Step 7: Report results, no commit**

This task validates the built plugin against the spec's Testing/Verification Plan. Nothing in this repo changes as a result unless a real bug is found, in which case return to the relevant task, fix it, and create a new commit.

---

## Self-Review

**Spec coverage:** `to-spec`, `to-ticket`, `tdd`, `to-implement`, `to-code-review`, and `quality-reviewer` each map to Tasks 3-7. The manifests and discovery mechanics map to Task 1 and Task 9. README and LICENSE map to Task 2 and Task 8. The spec's Testing/Verification Plan maps to Tasks 9-10. No spec section is without a task.

**Placeholder scan:** no TBD/TODO/"implement later" anywhere in this plan. The single placeholder value, `<your-github-username>`, is explicit and intentionally deferred until this repo has a GitHub remote, which is out of scope for this plan (noted in Task 1).

**Type/name consistency:** every skill's frontmatter `name` (`to-spec`, `to-ticket`, `tdd`, `to-implement`, `to-code-review`) and the agent's `name: quality-reviewer` are spelled identically everywhere they're referenced across tasks, and match the spec.

## Execution Options

**1. Subagent-driven** - a fresh subagent per task, with review between tasks.
**2. Inline** - execute tasks in this session, checkpoint after each.

Tasks 1-8 are small, self-contained file writes with lightweight YAML/JSON validation and no shared runtime state, so inline execution should be fast with little risk of losing context between them. Task 10, the end-to-end dry run, is the one place a fresh pair of eyes helps most, since it's exercising the pipeline rather than writing to it - worth considering as a separate subagent pass once Tasks 1-9 are done.

Which approach would you like, and should I start now?
