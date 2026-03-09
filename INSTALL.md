# Install

This repository is intended to be used as a reusable pi skill pack.

## Recommended setup

Create one symlink from your global skills directory to this repo's `skills/` directory:

```bash
mkdir -p ~/.agents/skills
ln -sfn /absolute/path/to/software-development-manifesto/skills ~/.agents/skills/software-development-manifesto
```

Example for this checkout:

```bash
mkdir -p ~/.agents/skills
ln -sfn /Users/trangelier/development/ai/software-development-manifesto/skills ~/.agents/skills/software-development-manifesto
```

This works because pi recursively discovers nested `SKILL.md` files under a skills directory.

## Why this layout

Using one symlink gives you:

- all repo skills available everywhere
- one shared source of truth under `skills/shared-ai/`
- no need to manage a separate symlink for each skill
- easy updates when the repo adds new skills

## What pi will discover

After linking, pi should discover these skills from this repo:

- `workflow-triage`
- `spec-to-beads`
- `implement-bead-task`
- `review-task`
- `bugfix-fast-path`
- `conventional-commit`

`skills/shared-ai/` is shared documentation only. It is not a skill and should not register as one.

## Reload pi

Inside pi, run:

```text
/reload
```

Or restart pi.

## Optional cleanup from older setup

If you previously created one symlink per skill, remove the old repo-specific symlinks so discovery stays tidy:

```bash
rm -f \
  ~/.agents/skills/spec-to-beads \
  ~/.agents/skills/implement-bead-task \
  ~/.agents/skills/conventional-commits \
  ~/.agents/skills/conventional-commit \
  ~/.agents/skills/review-task \
  ~/.agents/skills/bugfix-fast-path \
  ~/.agents/skills/workflow-triage
```

Then recreate the single pack symlink:

```bash
ln -sfn /absolute/path/to/software-development-manifesto/skills ~/.agents/skills/software-development-manifesto
```
