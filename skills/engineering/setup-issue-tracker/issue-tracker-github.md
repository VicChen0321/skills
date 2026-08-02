# Issue tracker: GitHub

Specs and tickets live as GitHub issues in this repo. Use the `gh` CLI for every operation - it infers the repo from `git remote -v` automatically.

## Conventions

- **Spec**: one issue, label `spec`. `gh issue create --title "[Spec] <topic>" --label spec --body "..."`, using a heredoc for a multi-line body.
- **Ticket**: one issue per ticket, label `ticket`. Body includes `Spec: #<spec-issue-number>` and, if applicable, `Blocked by: #<n>, #<n>`.
- **Status**: no separate status field - the issue's own state carries it.
  - Open, no extra label -> Todo
  - Open, label `status:in-review` -> In Review (added by `to-implement` via `gh issue edit --add-label status:in-review`)
  - Closed -> Done (`to-code-review` closes it via `gh issue close --comment "..."`, with both review reports in the closing comment)
- **Index**: none needed - `gh issue list --label ticket --state open` is always the live list.

## Operations

- Create: `gh issue create --title "..." --body "..." --label ...`
- Read: `gh issue view <number> --comments`
- List: `gh issue list --label ticket --state open --json number,title,body,labels`
- Comment: `gh issue comment <number> --body "..."`
- Label: `gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- Close: `gh issue close <number> --comment "..."`

## When a skill says "publish the spec" or "create a ticket"

Create a GitHub issue per the conventions above.

## When a skill says "find the relevant ticket"

`gh issue view <number> --comments`, or `gh issue list` filtered per the status conventions to find what's unblocked.
