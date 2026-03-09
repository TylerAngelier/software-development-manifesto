---
name: spec-to-beads
description: Draft durable implementation specs and translate approved work into Beads epics and tasks. Use when the user asks for `/spec`, wants a feature spec written or refined, or wants approved work turned into Beads issues with acceptance criteria, scope boundaries, and dependencies.
---

# Spec To Beads

Draft the spec as the durable contract, then translate the approved work into `bd` issues. Keep the task system in Beads, not in markdown.

## Use This Skill When

- the user explicitly asks for `/spec`
- the request is a feature, migration, or other non-trivial change that benefits from an approved contract
- architectural, security, rollout, or rollback decisions need to be made explicit before implementation

## Do Not Use This Skill When

- the request is a trivial fix or documentation cleanup
- the request is a bounded bug fix better handled by `bugfix-fast-path`
- the user only wants implementation of an already-approved Beads task

If the right rigor level is unclear, use `workflow-triage` first.

## Read These Files

Read only the files needed for the current request:

- `../shared-ai/templates/spec.md`
- `../shared-ai/commands/spec.md`
- `../shared-ai/specs/README.md`
- `../shared-ai/reference/codex-multi-agent.md`
- `../shared-ai/commands/implement-task.md`
- `../shared-ai/commands/conventional-commit.md`
- `../../AGENTS.md`

## Workflow

1. Read the user request and the minimum relevant repo context.
2. Draft the spec at `.ai/specs/<slug>.md` using the template at `../shared-ai/templates/spec.md`.
3. Make the contract explicit:
   - why the work exists
   - concrete deliverable
   - must / must not / out of scope
   - technical decisions and constraints
   - security, data, rollback, and cross-system impact where relevant
   - validation strategy
4. Stop for human review before creating Beads tasks or code.
5. After approval, create one Beads epic and the child tasks.
6. Put epic-level success criteria into the epic description before linting.
7. Link every task back to the spec with `--spec-id`.
8. Put acceptance criteria in `--acceptance` and implementation notes in `--design`.
9. Use dependencies instead of prose for task ordering.
10. Run `bd lint` where the issue type expects required sections.

## Multi-Agent Use

- The controller owns the final spec text.
- Use an optional `explorer` as `Spec Analyst` for focused codebase discovery.
- Use an optional `default` reviewer as `Spec Critic` to find ambiguity, weak validation, or missing constraints.
- If subagent support is unavailable, keep the same phases sequentially in the controller thread.

## Worked Examples

### Good Fit

> "Use `/spec` to plan a billing webhook retry system. We need retries, dead-letter handling, and rollout notes before building it."

Expected outcome:

- draft `.ai/specs/billing-webhook-retries.md`
- explicit constraints and rollout notes
- stop for human approval before task creation

### Poor Fit

> "Fix the typo in the README heading."

Better path:

- fix directly or use `workflow-triage`
- do not create a full spec

## Output

Return:

- the spec path
- the key decisions or open questions
- whether the spec is ready for human review
- after approval, the created Beads IDs and their purpose

## Boundaries

- Do not start implementation during `/spec` unless the user explicitly changes direction.
- Do not keep an authoritative markdown task list once Beads issues exist.
- Revise the spec when the contract changes, not just because task state changes.
