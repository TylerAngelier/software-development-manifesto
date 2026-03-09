# `/workflow-triage`

Use this runbook to classify a request and choose the minimum safe workflow.

## Goal

Turn the decision framework into an actionable routing decision.

## Inputs

- user request
- known constraints or affected systems
- urgency and risk cues

## Output

- classification
- recommended workflow
- reasoning
- open questions if classification is blocked

## Classification Ladder

1. **Trivial fix**
   - examples: typo, comment, docs wording
   - route: direct fix, then `/conventional-commit`
2. **Bug fix**
   - examples: regression, broken edge case, incorrect behavior
   - route: `/bugfix-fast-path`
3. **Small feature**
   - examples: bounded user-visible enhancement with clear scope
   - route: lightweight `/spec`, then `/implement-task`
4. **Complex feature**
   - examples: multi-step feature, architectural choice, migration, cross-system work
   - route: full `/spec`, human approval, then `/implement-task`
5. **Ambiguous / risky change**
   - route: stop and ask clarifying questions before selecting a workflow

## Risk Questions

Increase rigor if the answer may be yes:

- Does this touch user data?
- Is it security-related?
- Is it hard to roll back?
- Does it affect multiple systems?
- Is the scope unclear?
- Could it introduce regressions in shared code?

## Routing Rules

- Prefer the lightest workflow that still manages risk.
- When in doubt between bug fix and feature, ask what behavior should change and whether a durable spec is needed.
- When architectural or migration decisions appear, route to `/spec`.
- When the request is tiny and low-risk, do not force the full spec workflow.
