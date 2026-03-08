# `/conventional-commit`

Use this after `/implement-task` has finished for the current task.

## Goal

Create a small, traceable conventional commit for the completed task.

## Rules

- Use the existing `conventional-commits` skill.
- Commit only the work for the current task.
- Prefer one coherent commit per completed Beads task.
- Include Beads traceability in the subject or body when practical.
- The body must start with `Why:`.
- Do not create a dedicated commit summarizer agent. Use the finalized implementer or controller summary after review is complete.

## Preconditions

- the task has been implemented
- verification has run
- review findings have been handled
- the task is already closed in `bd`

## Suggested Shape

```text
feat(scope): short summary (bd-123)

Why: implement the approved task linked to .ai/specs/<slug>.md and close the corresponding Beads issue.
```

If the existing skill and the local repo convention disagree, follow the skill for commit formatting and keep the commit scoped to the task.
