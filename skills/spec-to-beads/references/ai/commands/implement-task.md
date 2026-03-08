# `/implement-task`

Use this runbook when the user wants one approved Beads task implemented.

## Goal

Implement a single task with a controller-led Codex multi-agent loop:

1. implement
2. review
3. fix
4. verify
5. close task

## Inputs

- target Beads issue ID, or `bd ready` output
- linked spec path
- task acceptance criteria

## Output

- completed code changes for one task
- verification results
- updated and closed Beads task
- ready-to-commit working tree

## Subagent Mapping

Use the definitions in `../reference/codex-multi-agent.md`.

- controller -> main thread
- optional focused codebase lookup -> `explorer`
- implementer -> `worker`
- reviewer -> `default`
- verification helper -> optional `explorer` or `default`

## Single-Checkout Rules

- Do not create git worktrees.
- Do not run parallel code-edit agents against the same task.
- Use subagents for role separation, not branch isolation.
- If subagent support is unavailable, keep the same phases and run them sequentially in the controller thread.

## Controller Workflow

1. Resolve the task.

```bash
bd ready --json
bd show <task-id>
```

2. Claim it.

```bash
bd update <task-id> --claim
```

3. Read:
   - the Beads issue
   - the linked spec
   - the relevant code and tests

4. Decide delegation:
   - implement locally for very small changes, then request review
   - or spawn one implementer agent for a bounded patch

5. Run task verification as soon as the patch exists.

6. Spawn a fresh reviewer agent with:
   - the task ID
   - the spec summary
   - the changed files or diff summary
   - verification output

   If subagent support is unavailable, do a dedicated review pass in the controller thread before moving to fixes.

7. Triage reviewer findings.

8. Fix accepted findings via the implementer or locally.

9. Rerun verification.

10. Update Beads notes if needed and close the task.

```bash
bd close <task-id> --reason "Completed"
```

## Implementer Prompt Shape

Use a bounded prompt:

- task ID and title
- exact files or module area
- linked spec path
- acceptance criteria
- verification command
- explicit scope exclusions

Expected implementer output:

- changed files
- what was implemented
- verification attempted
- unresolved risk or open question

## Reviewer Prompt Shape

Ask for a code-review result, not a rewrite:

- check behavior against the spec
- find bugs, regressions, edge cases, security issues, and missing tests
- call out scope drift
- prefer concrete findings over style comments

Expected reviewer output:

- prioritized findings
- missing tests
- scope drift
- explicit "no findings" if clean

## Exit Criteria

Do not close the task until:

- acceptance criteria are satisfied
- verification passes or failures are explicitly accepted by the user
- reviewer findings are addressed or consciously rejected

## Follow-On

Once the task is closed and the working tree is coherent, use `/conventional-commit`.
