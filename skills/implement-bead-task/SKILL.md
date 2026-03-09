---
name: implement-bead-task
description: Implement one approved Beads task with a controller-led review loop. Use when the user asks for `/implement-task`, wants a specific Beads issue claimed and completed, or wants one task executed against a linked spec with verification, review, and closure.
---

# Implement Bead Task

Claim one Beads task, implement it against the linked spec, run review and verification, then close the task. Keep scope tight and do not mix unrelated work into the same execution.

## Use This Skill When

- the user asks for `/implement-task`
- a Beads task is already approved and linked to a spec
- one bounded task should be implemented, reviewed, verified, and closed

## Do Not Use This Skill When

- the work still needs a spec or approval first
- the request is a small bug fix better handled by `bugfix-fast-path`
- the user wants a standalone review without implementation; use `review-task`

If the correct workflow is unclear, use `workflow-triage` first.

## Read These Files

Read only the files needed for the current request:

- `../shared-ai/commands/implement-task.md`
- `../shared-ai/reference/codex-multi-agent.md`
- `../shared-ai/commands/conventional-commit.md`
- `../shared-ai/commands/spec.md`
- `../shared-ai/templates/spec.md`
- `../shared-ai/specs/README.md`
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
5. Run the first round of task verification as soon as the patch exists.
6. Run a fresh review phase:
   - use a `default` reviewer when subagent support exists
   - otherwise run a separate controller-led review pass
7. Fix accepted findings.
8. Rerun verification.
9. Update Beads notes with verification evidence, manual checks, and any consciously accepted residual risk.
10. Close the task.
11. Hand off to the `conventional-commit` skill.

## Verification Expectations

Use the strongest available verification that fits the task:

1. targeted automated checks
2. new or updated tests when behavior changes and the repo supports them
3. broader safety checks for risky changes
4. focused manual validation for behavior not covered by automation
5. Beads notes that record what actually ran

If no automated tests exist, do not treat that as invisible. Either add the missing coverage or explicitly document why manual verification is the best available evidence.

## Multi-Agent Use

- The controller owns final judgment on review findings.
- Use an optional `explorer` for one focused codebase question if needed.
- Keep one write owner at a time for the active patch.
- Treat the reviewer as read-only.
- If subagent support is unavailable, keep the phases separate anyway: implement, review, fix, verify, close.

## Worked Examples

### Good Fit

> "Use `/implement-task` for BD-142. The spec is approved. Claim it, implement it, review it, and close it."

Expected outcome:

- one task claimed
- bounded patch
- verification and review recorded
- task closed only after checks pass

### Poor Fit

> "Please look over this diff and tell me if it is safe."

Better path:

- use `review-task`
- do not claim or close a Beads task unless implementation is also requested

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
