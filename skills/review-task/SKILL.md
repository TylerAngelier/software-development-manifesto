---
name: review-task
description: Run a fresh-context review of one Beads task, bugfix, or diff. Use when the user wants an independent review against the spec, acceptance criteria, changed files, or verification output without mixing review into implementation.
---

# Review Task

Perform a read-only review of one bounded unit of work. Prioritize bugs, regressions, edge cases, missing tests, weak verification, and scope drift.

## Use This Skill When

- the user asks for a review of a task, patch, or diff
- implementation already happened and a fresh-context review is needed
- you want review without claiming or closing a Beads task

## Do Not Use This Skill When

- the task still needs implementation; use `implement-bead-task` or `bugfix-fast-path`
- the request still needs classification; use `workflow-triage`
- the user wants a spec drafted; use `spec-to-beads`

## Read These Files

Read only the files needed for the current request:

- `../shared-ai/commands/review-task.md`
- `../shared-ai/reference/codex-multi-agent.md`
- `../shared-ai/templates/spec.md`
- `../../AGENTS.md`
- the Beads issue or bug summary when available
- the linked spec when available
- the changed files or diff summary
- verification output

## Workflow

1. Gather the task scope, spec, changed files, and verification output.
2. Run a fresh-context review focused on correctness, regressions, security, missing tests, weak verification, and scope drift.
3. Return prioritized findings only, or explicitly say `no findings`.
4. Do not edit code unless the controller explicitly changes roles.

## Worked Examples

### Good Fit

> "Use `review-task` on BD-142 after implementation. Here are the changed files and test results."

Expected outcome:

- prioritized findings or `no findings`
- comments tied to spec and verification evidence

### Poor Fit

> "Fix BD-142 and review it."

Better path:

- use `implement-bead-task`
- review should be part of the full task loop

## Output

Return:

- prioritized findings
- missing tests or verification concerns
- scope drift notes
- explicit `no findings` if clean
