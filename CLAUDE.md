# ai-sdlc repo conventions

Maintainer and structural conventions for this repo.
User-facing skill descriptions live in [README.md](README.md); this file covers how the repo is organized and kept consistent.

## Categories

Skills are organized by category, one subdirectory under `skills/` per category: `engineering/` and `productivity/`.
Every skill in either category ships in both plugins (see Distribution below).
There is no non-shipped bucket today.
Introduce one only when a real draft or deprecated skill needs excluding, not in advance.

## Naming

Pipeline-stage skills follow a `to-<noun>` naming convention, each name describing the artifact the skill produces from the previous stage's output.
`tdd`, `domain-modeling`, `interrogate-with-docs`, `resolving-merge-conflicts`, `setup-issue-tracker`, `codebase-design`, `improve-architecture`, and the `productivity/` skills are exceptions: technique and utility skills other work draws on, not stages that hand off an artifact to the next stage in sequence.

## Invocation mode

Every skill is either user-invoked (reachable only by the human typing its name) or model-invoked (reachable by model or user).
User-invoked skills set `disable-model-invocation: true` in `SKILL.md` frontmatter: currently `to-spec`, `to-ticket`, `to-implement`, `interrogate-with-docs`, `resolving-merge-conflicts`, `setup-issue-tracker`, `writing-great-skills`, and `improve-architecture`.
Everything else stays model-invoked, since something else needs to reach it directly from inside its own process, e.g. `to-implement` driving `tdd`, `to-code-review` firing off a ticket's status, or `improve-architecture` reaching for `codebase-design`'s vocabulary.

This repo supports three agent harnesses: Claude Code, Codex, and Pi.
Claude Code and Pi both read `disable-model-invocation` from `SKILL.md` frontmatter.
Every skill also carries an `agents/openai.yaml` beside its `SKILL.md`, holding Codex UI metadata (`interface.display_name`, `interface.short_description`).
A user-invoked skill's `agents/openai.yaml` sets `policy.allow_implicit_invocation: false`, Codex's analog of `disable-model-invocation: true`.
Keep the frontmatter and Codex policy in sync: a skill is user-invoked in all three harnesses or in none.

## Distribution

This repo ships in three harnesses:

- **Claude Code** - `.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json`.
  `skills` is `["./skills/engineering/", "./skills/productivity/"]`.
- **Codex** - `.codex-plugin/plugin.json` + `.agents/plugins/marketplace.json`.
  `skills` is `"./skills/"`; Codex's manifest only accepts a single path string, not an array.
- **Pi** - `package.json` is a Pi package manifest.
  `pi.skills` is `["./skills"]`, so users can install the repository with `pi install git:github.com/VicChen0321/skills`.

Each manifest points at category directories or their common `skills/` parent, not individual skills.
A common parent works because every skill under `skills/` ships, with no non-promoted bucket to exclude.
Adding a new skill under an existing category needs no manifest edit, only the new skill folder plus a README entry.
Bump `.claude-plugin/plugin.json`, `.codex-plugin/plugin.json`, `package.json`, and the Claude marketplace entry to the same version on any change that ships.
Claude Code uses the plugin version to decide when installed users see an update.

`AGENTS.md` is a symlink to this file, so Codex and Pi read the same conventions Claude Code does.

## Two different `agents/` folders

Don't confuse them:

- Repo-root `agents/` (`agents/quality-reviewer.md`, `agents/spec-reviewer.md`) holds Claude Code **subagents** and the review rubrics `to-code-review` uses for harnesses without subagents.
- Each skill's own `<skill>/agents/openai.yaml` holds **Codex metadata** for that one skill.

They share a folder name at different depths in the tree; they are not the same mechanism.
