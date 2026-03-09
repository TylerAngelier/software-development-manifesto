# `/conventional-commit`

Use this after `/implement-task` or `/bugfix-fast-path` has finished for the current unit of work.

## Goal

Create a small, traceable conventional commit for the completed work.

## Rules

- Use the `conventional-commit` skill.
- Commit only the work for the current task or bugfix scope.
- Prefer one coherent commit per completed Beads task or bounded bugfix.
- Include Beads traceability in the subject or body when practical.
- The body must start with `Why:`.
- Do not create a dedicated commit summarizer agent. Use the finalized implementer or controller summary after review is complete.

## Preconditions

- the task or bugfix has been implemented
- verification has run
- review findings have been handled
- if using Beads, the task is already closed in `bd`

## Suggested Shape

```text
feat(scope): short summary (bd-123)

Why: implement the approved task linked to .ai/specs/<slug>.md and close the corresponding Beads issue.
```

If the existing conventional commit standard and the local repo convention disagree, follow the `conventional-commit` skill for commit formatting and keep the commit scoped to the finished unit of work.
