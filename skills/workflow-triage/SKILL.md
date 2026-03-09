---
name: workflow-triage
description: Assess a request against the decision framework and route it to the appropriate workflow with the minimum safe rigor. Use when it is unclear whether the work should be a trivial fix, bugfix, spec-driven feature, or Beads task execution.
---

# Workflow Triage

Turn the decision framework into an actionable routing decision before work starts.

## Use This Skill When

- the request is ambiguous
- you need to choose between direct work, `bugfix-fast-path`, `spec-to-beads`, or `implement-bead-task`
- you want a fast classification with explicit reasoning

## Do Not Use This Skill When

- the user has already clearly chosen the workflow and the fit is obvious
- implementation is already underway and the scope is settled

## Read These Files

Read only the files needed for the current request:

- `../shared-ai/commands/workflow-triage.md`
- `../../DECISION-FRAMEWORK.md`
- `../../AGENTS.md`
- any directly relevant repo context needed to classify the request

## Workflow

1. Read the request and any obvious repo context.
2. Classify the work: trivial fix, bug fix, small feature, complex feature, or ambiguous/risky.
3. Ask the risk questions from the decision framework.
4. Recommend the next workflow and explain why.
5. If the classification is blocked, ask the minimum clarifying questions.

## Worked Examples

### Good Fit

> "We need to change authentication and also support enterprise SSO. What workflow should we use?"

Expected outcome:

- classify as complex feature
- route to `spec-to-beads`
- explain the security and rollback reasons

### Good Fit

> "There is a typo in the settings page label."

Expected outcome:

- classify as trivial fix
- recommend direct fix and `conventional-commit`

### Poor Fit

> "Implement BD-142 now."

Better path:

- use `implement-bead-task`
- triage is unnecessary if the workflow is already explicit

## Output

Return:

- classification
- recommended workflow
- concise reasoning
- open questions if needed
