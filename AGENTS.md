# Repository Instructions

## Purpose

This repo defines a single-checkout workflow for building software with AI like a disciplined engineer:

- classify the request to the right rigor level
- draft a spec when needed
- get human approval
- translate approved work into `bd` tasks
- implement one task at a time
- run an independent review loop
- commit with conventional commits

The `/workflow-triage`, `/spec`, `/implement-task`, `/review-task`, `/bugfix-fast-path`, and `/conventional-commit` labels below describe workflow entrypoints. Treat them as prompt/runbook names unless the surrounding toolchain explicitly wires them up as native commands.

## Non-Negotiables

- Do not use git worktrees in this workflow.
- Use `bd` for live task tracking. Do not maintain an authoritative markdown task list after Beads issues exist.
- Treat `.ai/specs/*.md` as the durable source of truth for approved intent and constraints.
- Work on exactly one implementation task at a time in a checkout.
- Keep context fresh. Start each task from the Beads issue and linked spec instead of relying on a long prior chat.
- Do not commit code that has not passed the task verification steps.

## Task Classification

Not all tasks require the full workflow. Use `DECISION-FRAMEWORK.md` or `/workflow-triage` to determine the appropriate level of rigor.

Quick guidance:
- **Trivial fixes** (typos, comments): Fix directly, then commit
- **Bug fixes**: Use `/bugfix-fast-path`
- **Features**: Use `/spec` with depth proportional to complexity, then `/implement-task`
- **Standalone review**: Use `/review-task`

When in doubt, start with more rigor. It is easier to reduce ceremony than to add missing controls retroactively.

## AI at Every Stage

AI accelerates the entire software development lifecycle, not just coding:

1. **Requirements** — Draft PRDs, challenge assumptions, prototype to test ideas
2. **Technical Design** — Think through tradeoffs, document decisions
3. **Task Breakdown** — Break specs into bounded tasks with context
4. **Build** — Write code with proper briefing
5. **Review** — Self-review catches issues missed during generation
6. **Deploy** — Set up CI/CD and infrastructure
7. **Monitor** — Configure alerts, error tracking, logging

## Beads Rules

- Use `bd ready` to find unblocked work.
- Claim the task before implementation: `bd update <id> --claim`.
- Every implementation task should link back to a spec with `--spec-id`.
- Put acceptance criteria in the issue with `--acceptance`.
- Put execution notes or technical caveats in `--design` or `--notes`.
- Record meaningful verification evidence in `--notes` before closing the task.
- Use dependencies instead of prose to represent ordering.
- Close the task only after verification and review are complete.

## Roles

Detailed role definitions and command mappings live in `skills/shared-ai/reference/codex-multi-agent.md`.

### Controller

The main Codex thread is the controller. It owns:

- task selection and claim
- reading the spec and repo context
- deciding what to delegate
- integrating outcomes from subagents
- running verification
- updating and closing the Beads issue

### Implementer

The implementer agent writes or changes code for the claimed task only.

- Keep scope limited to the current Beads issue or bounded bugfix.
- Follow the linked spec, existing patterns, and acceptance criteria.
- Report what changed, what was verified, and any remaining risk.

### Reviewer

The reviewer agent is a fresh-context reviewer.

- Review against the spec, acceptance criteria, diff, and test results.
- Prioritize bugs, regressions, edge cases, missing tests, weak verification, and scope drift.
- Do not become a second implementer unless the controller explicitly requests it.

## Codex Agent Types

Use the current Codex subagent types deliberately:

- `explorer`: focused read-only codebase discovery
- `worker`: bounded implementation with explicit file ownership
- `default`: review, critique, synthesis, or summary work

Do not use a spawned subagent when the controller can complete the work more directly on the critical path.

## Multi-Agent Rules

- Use Codex subagents for bounded support, not for uncontrolled fan-out.
- Keep one coding owner at a time for the active patch.
- The reviewer should normally be read-only.
- If a delegated task is on the critical path, keep it narrow and wait only when blocked.
- Do not spawn parallel code-edit agents against the same files in this single-checkout workflow.
- Prefer a controller-led sequence: implement -> review -> fix -> verify.
- If subagent support is unavailable, preserve the same phases sequentially in the controller thread.

## `/workflow-triage`

When the user asks for `/workflow-triage`:

1. Read the request and enough repo context to classify it.
2. Apply the questions in `DECISION-FRAMEWORK.md`.
3. Route to the lightest safe workflow.
4. Ask clarifying questions if classification is blocked.

## `/spec`

When the user asks for `/spec`:

1. Read the relevant code and constraints.
2. Draft a spec from `skills/shared-ai/templates/spec.md`.
3. Save it under `.ai/specs/<slug>.md`.
4. Stop for human review before creating tasks or code.
5. After approval, create one epic and the child tasks in `bd`.
6. Ensure the canonical task list lives in `bd`, not in the spec file.

Typical subagents:

- optional `explorer` as spec analyst
- optional `default` as spec critic

Use concise, testable task titles and acceptance criteria.

## `/implement-task`

When the user asks for `/implement-task`:

1. Resolve the target Beads issue, or choose from `bd ready`.
2. Read the issue, linked spec, and relevant code.
3. Claim the issue.
4. Implement the change locally or via one implementer subagent.
5. Run task verification.
6. Ask a fresh reviewer subagent to review the result when available. Otherwise run a separate controller-led review pass before fixing.
7. Fix accepted findings.
8. Rerun verification.
9. Update Beads notes with verification evidence if needed and close the issue.

The controller owns final judgment on reviewer findings.

Typical subagents:

- optional `explorer` for one focused codebase question
- optional `worker` as implementer
- `default` reviewer for fresh-context review

## `/review-task`

When the user asks for `/review-task`:

1. Gather the task, spec, changed files, and verification evidence.
2. Run a fresh-context review.
3. Return prioritized findings or explicit approval.
4. Keep the reviewer read-only unless the user changes scope.

## `/bugfix-fast-path`

When the user asks for `/bugfix-fast-path`:

1. Confirm the request is a bounded bug fix.
2. Clarify expected and broken behavior.
3. Implement the smallest effective fix.
4. Verify, review, fix, and verify again.
5. Hand off to `/conventional-commit`.

Escalate to `/spec` if the work turns into a feature, migration, or architecture decision.

## `/conventional-commit`

When the user asks for `/conventional-commit`:

- Use the `conventional-commit` skill.
- Keep the commit scoped to the current task or bugfix.
- Include Beads traceability in the message where practical.
- The commit body must start with `Why:`.

## Files To Know

- `skills/shared-ai/templates/spec.md`
- `.ai/specs/`
- `skills/shared-ai/reference/codex-multi-agent.md`
- `skills/shared-ai/commands/workflow-triage.md`
- `skills/shared-ai/commands/spec.md`
- `skills/shared-ai/commands/implement-task.md`
- `skills/shared-ai/commands/review-task.md`
- `skills/shared-ai/commands/bugfix-fast-path.md`
- `skills/shared-ai/commands/conventional-commit.md`
- `skills/workflow-triage/SKILL.md`
- `skills/spec-to-beads/SKILL.md`
- `skills/implement-bead-task/SKILL.md`
- `skills/review-task/SKILL.md`
- `skills/bugfix-fast-path/SKILL.md`
- `skills/conventional-commit/SKILL.md`
