# `/bugfix-fast-path`

Use this runbook for a bounded bug fix that does not justify a full feature spec.

## Goal

Apply the minimum safe workflow for bug fixes:

1. clarify the bug
2. reproduce or identify the failing behavior
3. implement the smallest effective fix
4. verify
5. review
6. verify again
7. hand off to `/conventional-commit`

## Inputs

- bug report or user request
- reproduction steps when available
- relevant code and tests

## Output

- changed files
- bug fixed or narrowed
- verification results
- review findings and dispositions
- ready-to-commit working tree

## Workflow

1. Confirm this is a bug fix, not a feature request in disguise.
2. Capture the expected behavior and current wrong behavior.
3. Read the relevant code and tests.
4. Implement the smallest patch that fixes the bug.
5. Run targeted verification immediately.
6. Run a fresh review pass.
7. Fix accepted findings.
8. Rerun verification.
9. Summarize residual risk and hand off to `/conventional-commit`.

## Verification Expectations

- Prefer a reproduction-backed test when the bug is testable.
- If no automated test is practical, explain why and perform focused manual validation.
- Run broader checks when the bug touches shared or risky code.

## Stop and Escalate

Stop and suggest `/spec` instead when:

- the request changes product behavior beyond a bug fix
- the scope crosses multiple systems with unclear contracts
- the fix requires a migration or architectural decision
- rollback or security impact is unclear
