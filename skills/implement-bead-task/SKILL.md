---
name: implement-bead-task
description: Implement one approved Beads task with a controller-led review loop. Use when the user asks for `/implement-task`, wants a specific Beads issue claimed and completed, or wants one task executed against a linked spec with verification, review, and closure.
---

# Implement Bead Task

Claim one Beads task, implement it against the linked spec, run review and verification, then close the task. Keep scope tight and do not mix unrelated work into the same execution.

## Read These Files

Read only the files needed for the current request:

- `references/ai/commands/implement-task.md`
- `references/ai/reference/codex-multi-agent.md`
- `references/ai/commands/conventional-commit.md`
- `references/ai/commands/spec.md`
- `references/ai/templates/spec.md`
- `references/ai/specs/README.md`
- `../../AGENTS.md`
- the Beads issue resolved from an explicit ID or `bd ready`
- the linked spec in `.ai/specs/` (repo-level specs directory)
- the relevant implementation files in the repo

## Workflow

1. Resolve the target task from an explicit ID or `bd ready`.
2. Read the Beads issue, linked spec, and relevant code.
3. Claim the task with `bd update <id> --claim`.
4. Decide delegation:
   - implement locally for a very small change
   - or use one `worker` implementer for the bounded patch
5. Run the task verification as soon as the patch exists.
6. Run a fresh review phase:
   - use a `default` reviewer when subagent support exists
   - otherwise run a separate controller-led review pass
7. Fix accepted findings.
8. Rerun verification.
9. Update Beads notes if needed and close the task.
10. Hand off to the existing conventional commit workflow.

## Multi-Agent Use

- The controller owns final judgment on review findings.
- Use an optional `explorer` for one focused codebase question if needed.
- Keep one write owner at a time for the active patch.
- Treat the reviewer as read-only.
- If subagent support is unavailable, keep the phases separate anyway: implement, review, fix, verify, close.

## Output

Return:

- the task ID
- changed files
- what was implemented
- verification results
- review findings and dispositions
- whether the Beads task was closed

## Boundaries

- Do not implement more than one Beads task in a single run unless the user explicitly changes scope.
- Do not close the task until verification passes or the user explicitly accepts a failed verification result, and review findings are addressed or consciously rejected.
- Do not create a separate commit summarizer agent. Reuse the finalized implementer or controller summary after review for commit handoff.
