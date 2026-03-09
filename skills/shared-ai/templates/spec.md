# Feature Name

> Canonical task tracking lives in Beads. This spec defines the contract for the work. After approval, create or update the implementation tasks in `bd` and keep task state there.

## Status

Draft | Approved | Superseded

## Why

[1-2 sentences on the problem being solved and why it matters now.]

## What

[Concrete deliverable. Specific enough to verify when the work is done.]

## Constraints

### Must

- [Required architecture, libraries, patterns, interfaces, or operational limits]

### Must Not

- [Explicit non-goals and forbidden approaches]

### Out Of Scope

- [Adjacent work that is intentionally excluded]

## Current State

- Relevant files:
- Existing patterns to follow:
- Known gaps or quirks:

## Technical Decisions

- Decision:
  Reason:
- Decision:
  Reason:

## Impact Assessment

### Security / Privacy

- [Auth, secrets, permissions, data exposure, abuse cases, privacy concerns]

### Data / Migration

- [Schema changes, backfills, irreversible writes, data retention, compatibility]

### Cross-System Dependencies

- [External services, downstream consumers, deployment ordering, contracts]

### Rollback / Reversibility

- [How to undo safely, what makes rollback hard, fallback path]

## Risks

- [Main delivery or regression risks]

## Validation

### Automated Checks

- [Unit, integration, lint, typecheck, build, migration validation]

### Manual Checks

- [Human validation steps, exploratory scenarios, approval gates]

### Observability / Monitoring

- [Logs, metrics, alerts, dashboards, Sentry checks, post-release validation]

## Launch / Rollout Notes

- [Feature flags, staged rollout, migration sequencing, communication]

## Beads Translation

### Epic

- Title:
- Type: `epic`
- Spec path:
- Success criteria:

### Task Seeding Notes

- Capture the intended task split at a high level only.
- After approval, create the actual tasks in `bd` and keep their status, acceptance criteria, and dependencies there.
- If the task breakdown changes during implementation, update `bd` first.

### Notes

- Keep the canonical task list in `bd`, not in this file.
- When tasks change during implementation, update `bd` first and only revise this spec if the contract changed.
