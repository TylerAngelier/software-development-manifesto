---
name: spec-to-beads
description: Draft durable implementation specs and translate approved work into Beads epics and tasks. Use when the user asks for `/spec`, wants a feature spec written or refined, or wants approved work turned into Beads issues with acceptance criteria, scope boundaries, and dependencies.
---

# Spec To Beads

Draft the spec as the durable contract, then translate the approved work into `bd` issues. Keep the task system in Beads, not in markdown.

## Read These Files

Read only the files needed for the current request:

- `references/ai/templates/spec.md`
- `references/ai/commands/spec.md`
- `references/ai/specs/README.md`
- `references/ai/reference/codex-multi-agent.md`
- `references/ai/commands/implement-task.md`
- `references/ai/commands/conventional-commit.md`
- `../../AGENTS.md`

## Workflow

1. Read the user request and the minimum relevant repo context.
2. Draft the spec at `.ai/specs/<slug>.md` using the template at `references/ai/templates/spec.md`.
3. Make the contract explicit:
   - why the work exists
   - concrete deliverable
   - must / must not / out of scope
   - technical decisions and constraints
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
