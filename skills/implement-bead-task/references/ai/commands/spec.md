# `/spec`

Use this runbook when the user wants a spec drafted before implementation.

## Goal

Produce a durable, reviewable spec and then translate the approved work into Beads issues.

## Inputs

- user request
- relevant code and docs
- existing architectural constraints

## Output

- `.ai/specs/<slug>.md`
- after human approval: one Beads epic plus child tasks with acceptance criteria and dependencies

## Subagent Mapping

Use the definitions in `../reference/codex-multi-agent.md`.

- `Spec Analyst` -> optional `explorer`
- `Spec Critic` -> optional `default`
- controller -> always the main thread

## Workflow

1. Read the minimum code and docs needed to understand the change.
2. Draft the spec from `../templates/spec.md`.
3. Make the key decisions explicit:
   - what will be built
   - what will not be built
   - what patterns and files must be followed
   - how success will be verified
4. Save the spec under `.ai/specs/`.
5. Ask for human review before creating tasks or code.
6. After approval, create the Beads epic and child tasks.
7. Run `bd lint` on the new issues if the issue type requires required sections.

Keep the controller responsible for the final spec text. Subagents inform the spec; they do not own it.

## Translation Rules

- The spec is the contract.
- Beads is the live execution system.
- Do not maintain a second canonical task list in markdown after task creation.
- Use dependencies for order, not prose.

## Suggested Beads Shape

```bash
bd create "Feature: <name>" \
  --type epic \
  --spec-id ".ai/specs/<slug>.md" \
  --description $'## Success Criteria\n- <feature-level exit criteria>'

bd create "<task title>" \
  --type task \
  --parent <epic-id> \
  --spec-id ".ai/specs/<slug>.md" \
  --acceptance "<clear exit criteria>" \
  --design "<technical constraints or file guidance>"
```

For epic linting, include a `Success Criteria` section. For task or feature linting, include acceptance criteria before running `bd lint`.

## Stop Conditions

Stop and ask for review when:

- the spec introduces a new architectural decision
- the scope is ambiguous
- the validation strategy is unclear

Do not start implementation during `/spec` unless the user explicitly changes direction.
