# AI Software Development Manifesto

This repo packages a pragmatic AI-assisted engineering workflow:

- Specs are durable contracts.
- `bd` (Beads) is the live task system.
- Codex runs a controller-led multi-agent loop for implementation and review.
- Work happens one task at a time in a single local checkout.

This is intentionally stricter than "vibe coding". The goal is to keep the speed benefits of AI while preserving normal engineering controls: explicit decisions, bounded tasks, verification, review, and coherent commits.

The `/spec`, `/implement-task`, and `/conventional-commit` names in this repo are workflow labels backed by prompt/runbook files. They are not a claim that Codex will automatically register these files as native slash commands.

## Workflow

### 1. `/spec`

Draft a spec under `.ai/specs/<slug>.md` using [skills/spec-to-beads/references/ai/templates/spec.md](skills/spec-to-beads/references/ai/templates/spec.md). Directory guidance lives in [skills/spec-to-beads/references/ai/specs/README.md](skills/spec-to-beads/references/ai/specs/README.md).

The spec is the durable source of truth for:

- why the change exists
- what is in scope
- what must not happen
- technical decisions and constraints
- validation expectations

After human review and approval, convert the approved work into `bd` issues. Do not maintain the task list in markdown after that point.

Detailed runbook: [skills/spec-to-beads/references/ai/commands/spec.md](skills/spec-to-beads/references/ai/commands/spec.md)

### 2. `/implement-task`

Implement exactly one Beads task at a time.

The controller agent owns the session and:

- claims the task
- loads the linked spec
- coordinates an implementer agent when the task benefits from delegation
- requests a fresh review from a separate reviewer agent when subagent support is available
- applies or directs fixes
- reruns verification
- updates and closes the Beads task

This repo does not use worktrees. Multi-agent support is used for role separation inside one task workflow, not for parallel branch development.

If the active Codex surface does not support spawning subagents, keep the same phases sequentially in the controller thread.

Detailed runbook: [skills/implement-bead-task/references/ai/commands/implement-task.md](skills/implement-bead-task/references/ai/commands/implement-task.md)

### 3. `/conventional-commit`

Once the task is complete and verified, commit with the existing `conventional-commits` skill. Keep commits small, task-scoped, and traceable to the relevant Beads issue.

Workflow note: [skills/implement-bead-task/references/ai/commands/conventional-commit.md](skills/implement-bead-task/references/ai/commands/conventional-commit.md)

## Files

- [AGENTS.md](AGENTS.md): repo-level operating rules for Codex
- [skills/spec-to-beads/references/ai/templates/spec.md](skills/spec-to-beads/references/ai/templates/spec.md): durable spec template
- [skills/spec-to-beads/references/ai/specs/README.md](skills/spec-to-beads/references/ai/specs/README.md): where approved specs live
- [skills/spec-to-beads/references/ai/reference/codex-multi-agent.md](skills/spec-to-beads/references/ai/reference/codex-multi-agent.md): subagent definitions and command mappings
- [skills/spec-to-beads/references/ai/commands/spec.md](skills/spec-to-beads/references/ai/commands/spec.md): draft-and-translate runbook
- [skills/implement-bead-task/references/ai/commands/implement-task.md](skills/implement-bead-task/references/ai/commands/implement-task.md): controller/implementer/reviewer runbook
- [skills/implement-bead-task/references/ai/commands/conventional-commit.md](skills/implement-bead-task/references/ai/commands/conventional-commit.md): commit handoff notes
- [skills/spec-to-beads/SKILL.md](skills/spec-to-beads/SKILL.md): skill form of `/spec`
- [skills/implement-bead-task/SKILL.md](skills/implement-bead-task/SKILL.md): skill form of `/implement-task`

## Design Choices

- The spec stays in the repo because decisions need durable history.
- Tasks live in `bd` because status, dependencies, claiming, and acceptance criteria are operational state.
- The controller/implementer/reviewer split is deliberate. Generation and review are different tasks.
- Codex multi-agent support is documented explicitly in [skills/spec-to-beads/references/ai/reference/codex-multi-agent.md](skills/spec-to-beads/references/ai/reference/codex-multi-agent.md).
- Fresh context per task is mandatory. Long agent sessions degrade quality.
- No worktrees. Single-checkout discipline reduces local state complexity for solo task execution.

## Sources That Informed This Repo

- Owain Lewis, "Stop Vibe Coding — How to Build Software With AI Like a Senior Engineer"
- Owain Lewis, "Spec-Driven Development"
- OpenAI Codex app and Codex cloud materials on multi-agent workflows, AGENTS.md, and task-queue usage
- Beads documentation for issue structure, dependencies, and agent-oriented task tracking
