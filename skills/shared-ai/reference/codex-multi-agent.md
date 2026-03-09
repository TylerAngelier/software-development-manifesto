# Codex Multi-Agent Support

This document defines how this repo uses Codex subagents to support the workflow commands in a single local checkout.

## Scope

OpenAI positions Codex as a multi-agent system with isolated task execution and parallel work support. This repo intentionally uses a narrower pattern:

- one active implementation task at a time
- one local checkout
- one controller thread
- subagents for role separation, not branch isolation

We do not use git worktrees in this workflow.

## Codex Capability Assumption

This workflow is written for Codex surfaces that support subagent delegation.

Where that capability exists, the controller can use coordination primitives such as:

- spawning a bounded subagent for a specific task
- following up with an existing subagent
- waiting only when blocked on the result
- closing the subagent once its work is integrated

If the active Codex surface does not expose subagent support, keep the same role boundaries but execute them sequentially in the controller thread.

## Current Codex Agent Types

### Controller

The controller is the main Codex thread, not a spawned agent.

Responsibilities:

- read the user request
- load repo context
- decide whether delegation is useful
- keep the critical path moving
- run verification
- integrate subagent output

### `explorer`

Use `explorer` for focused, read-only codebase questions.

Good fits:

- "Which files implement auth middleware today?"
- "What patterns already exist for API validation?"
- "What tests currently cover this behavior?"

Do not use `explorer` for code edits.

### `worker`

Use `worker` for bounded implementation work with explicit file ownership.

Good fits:

- add or update one feature slice
- make a targeted patch
- write or update tests for one task

Rules:

- give the worker a concrete patch scope
- name the files or module area it owns
- assume it is not alone in the codebase
- do not assign overlapping write ownership in this single-checkout workflow

### `default`

Use `default` when the subtask is neither a pure codebase lookup nor a bounded code patch.

Good fits:

- review a diff against a spec
- critique a draft spec
- summarize open risks

## Repo-Defined Subagent Roles

### Spec Analyst

- Codex type: `explorer`
- Used by: `/spec`
- Purpose: gather the minimum codebase context needed to draft the spec
- Input: feature request, known files or subsystems
- Output: relevant files, existing patterns, constraints, unknowns

### Spec Critic

- Codex type: `default`
- Used by: `/spec`
- Purpose: review the drafted spec for ambiguity, missing constraints, and weak validation
- Input: draft spec, repo context summary
- Output: concrete gaps or approval

### Implementer

- Codex type: `worker`
- Used by: `/implement-task` and `/bugfix-fast-path`
- Purpose: make the bounded patch for the current Beads task or bugfix scope
- Input: task ID or bug summary, linked spec when present, acceptance criteria or exit criteria, file ownership, verification command
- Output: changed files, verification attempted, unresolved risks

### Reviewer

- Codex type: `default`
- Used by: `/implement-task`, `/review-task`, `/bugfix-fast-path`
- Purpose: perform fresh-context review of the implemented change
- Input: task summary, spec summary, diff summary, test or verification results
- Output: prioritized findings only, not a rewrite

### Verification Helper

- Codex type: `explorer` or `default`
- Used by: `/implement-task` or `/bugfix-fast-path` only when needed
- Purpose: diagnose a failing verification step or unclear test result
- Input: failing command output, relevant files
- Output: likely cause and targeted next action

## Single-Checkout Rules

- Only one write-owning agent at a time for the active patch.
- The reviewer is read-only unless the controller explicitly changes the plan.
- Do not run parallel code-edit agents against the same task.
- If a task is tiny, the controller may implement locally and only spawn the reviewer.
- Use subagents to sharpen roles, not to create hidden workflow complexity.

## Fallback Without Subagent Support

If the active Codex surface cannot spawn subagents:

- the controller still owns the task
- the controller performs the implementer step directly
- the controller performs a distinct review pass after implementation
- the review pass should be treated as a separate phase, not blended into coding
- human review becomes more important for risky or architectural changes

The workflow remains:

1. implement
2. review
3. fix
4. verify
5. close or hand off

## Command Mapping

### `/spec`

Recommended sequence:

1. Controller reads the request and relevant files.
2. Optional `Spec Analyst` gathers focused codebase context.
3. Controller drafts `.ai/specs/<slug>.md`.
4. Optional `Spec Critic` reviews the draft for missing constraints or weak validation.
5. Human reviews and approves the spec.
6. Controller creates the Beads epic and child tasks.

### `/implement-task`

Recommended sequence:

1. Controller resolves and claims the Beads task.
2. Controller loads the linked spec and relevant code.
3. Optional `explorer` answers one focused codebase question if needed.
4. `Implementer` makes the bounded patch.
5. Controller runs verification.
6. `Reviewer` reviews the result in fresh context.
7. Controller triages findings.
8. `Implementer` or controller fixes accepted findings.
9. Controller reruns verification and closes the task.

### `/review-task`

Recommended sequence:

1. Controller gathers the Beads issue, linked spec, diff summary, and verification output.
2. `Reviewer` performs a read-only review.
3. Controller triages findings and decides next actions.

### `/bugfix-fast-path`

Recommended sequence:

1. Controller confirms the issue is a bounded bug fix.
2. Controller gathers reproduction and affected code.
3. Optional `Implementer` makes the patch.
4. Controller runs verification.
5. `Reviewer` reviews the fix in fresh context.
6. Controller fixes accepted findings and reruns verification.
7. Controller hands off to `/conventional-commit`.

### `/conventional-commit`

Recommended sequence:

1. Controller confirms the task is closed or the bugfix is otherwise complete and verification is green.
2. Implementer or controller reuses the finalized summary from the review-complete state.
3. Controller uses the `conventional-commit` skill to create the commit.

## Output Contracts

Keep subagent responses structured and brief.

### Spec Analyst

- relevant files
- existing patterns
- constraints
- open questions

### Spec Critic

- blocking gaps
- non-blocking improvements
- explicit approval if no gaps remain

### Implementer

- files changed
- what was implemented
- verification attempted
- unresolved risk or questions

### Reviewer

- prioritized findings
- missing tests
- scope drift
- explicit "no findings" if clean

## Skill Conversion Notes

When these runbooks become skills:

- keep the role names stable
- import this document as the reference for delegation decisions
- do not hard-code worktree assumptions
- keep controller authority explicit
- keep reviewer and implementer responsibilities separate
