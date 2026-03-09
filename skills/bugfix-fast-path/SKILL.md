---
name: bugfix-fast-path
description: "Handle a bounded bug fix with the minimum safe workflow: clarify, implement, review, verify, and hand off to commit. Use when the request is clearly a bug fix and does not justify a full feature spec."
---

# Bugfix Fast Path

Use the lightest workflow that still preserves engineering discipline for bug fixes.

## Use This Skill When

- the request is a bounded bug fix
- expected behavior is already known or can be clarified quickly
- a full feature spec would be unnecessary overhead

## Do Not Use This Skill When

- the request introduces new product behavior or broad scope; use `spec-to-beads`
- the work is already captured as an approved Beads task; use `implement-bead-task`
- the classification is unclear; use `workflow-triage`

## Read These Files

Read only the files needed for the current request:

- `../shared-ai/commands/bugfix-fast-path.md`
- `../shared-ai/reference/codex-multi-agent.md`
- `../shared-ai/commands/conventional-commit.md`
- `../../AGENTS.md`
- the relevant code and tests

## Workflow

1. Clarify the expected behavior and current failure.
2. Read the relevant code and any existing tests.
3. Implement the smallest effective fix.
4. Run targeted verification immediately.
5. Run a fresh review pass.
6. Fix accepted findings.
7. Rerun verification.
8. Summarize residual risk and hand off to `conventional-commit`.

## Verification Expectations

- Prefer a failing or reproduction-backed automated test when practical.
- If automation is not practical, explain why and perform focused manual validation.
- Escalate to a heavier workflow when the bug exposes unclear contracts, migrations, or architectural decisions.

## Worked Examples

### Good Fit

> "The login form crashes when the optional avatar field is null. Fix it and verify the regression is covered."

Expected outcome:

- bounded patch
- targeted verification
- review pass before commit

### Poor Fit

> "Add SSO login and fix our auth problems."

Better path:

- use `workflow-triage`
- likely route to `spec-to-beads`

## Output

Return:

- changed files
- what was fixed
- verification results
- review findings and dispositions
- residual risk
