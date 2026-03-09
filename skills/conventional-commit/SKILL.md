---
name: conventional-commit
description: "Create git commits using the Conventional Commits specification, with a required body that starts with `Why: ...`. Use whenever the agent is about to run `git commit`, create a commit, split work into commits, or write or rewrite a commit message. Prefer small, coherent, working commits and avoid bundling unrelated changes together."
---

# Conventional Commit Skill

## Announce at start

When you activate this skill, **say exactly**:

> "I'm using the conventional-commit skill to create properly formatted commit messages."

## Use This Skill When

- a task or bugfix is finished and ready to commit
- the user asks for `/conventional-commit`
- the agent is about to run `git commit`
- the current diff may need to be split into smaller coherent commits

## Do Not Use This Skill When

- the work has not been verified yet
- review findings are still unresolved
- the diff mixes unrelated changes that should be separated first

If the workflow itself is unclear, use `workflow-triage` or finish `/implement-task` or `bugfix-fast-path` first.

## Commit Size Policy

When preparing commits:

1. Commit the smallest coherent working change that leaves the tree in a sensible state
2. Prefer multiple small commits over one large mixed commit when the work naturally splits
3. Do not combine unrelated refactors, fixes, docs, or formatting changes into a single commit
4. If the current diff is too broad, stage or select a smaller logical subset before committing
5. Ensure each commit is working at the level you can reasonably verify in the current environment
6. If a requested change is not yet in a working state, finish the minimal viable slice before committing instead of snapshotting broken intermediate work

Treat "small working commits" as part of using this skill, not as an optional preference.

## Commit Message Format

All commit messages MUST follow this structure:

```
<type>[optional scope]: <description>

Why: <reason>

[optional extra context]

[optional footer(s)]
```

## Required Types

| Type | Description | SemVer Impact |
|------|-------------|---------------|
| `feat` | A new feature | MINOR |
| `fix` | A bug fix | PATCH |
| `build` | Changes that affect the build system or external dependencies | - |
| `chore` | Other changes that don't modify src or test files | - |
| `ci` | Changes to CI configuration files and scripts | - |
| `docs` | Documentation only changes | - |
| `perf` | A code change that improves performance | - |
| `refactor` | A code change that neither fixes a bug nor adds a feature | - |
| `revert` | Reverts a previous commit | - |
| `style` | Changes that do not affect the meaning of the code (white-space, formatting, etc.) | - |
| `test` | Adding missing tests or correcting existing tests | - |

## Scope (Optional)

A scope provides additional context and MUST be a noun describing a section of the codebase:

```
feat(parser): add ability to parse arrays
fix(api): handle timeout errors gracefully
docs(readme): update installation instructions
```

## Breaking Changes

Breaking changes MUST be indicated in one of two ways:

### Option 1: `!` before the colon

```
feat!: remove legacy API endpoints
feat(api)!: change authentication flow
```

### Option 2: `BREAKING CHANGE:` footer

```
feat: remove legacy API endpoints

Why: Remove the deprecated API surface before the v2-only rollout to avoid split maintenance.

BREAKING CHANGE: The /v1/* endpoints have been removed. Migrate to /v2/* endpoints.
```

**Note:** `BREAKING CHANGE` MUST be uppercase. `BREAKING-CHANGE` is also acceptable.

## Description Rules

1. MUST immediately follow the colon and space after type/scope
2. MUST be a short summary of the code changes
3. SHOULD be in imperative mood ("add feature" not "added feature")
4. SHOULD NOT end with a period

## Body (Required)

Every commit message MUST include a body.

1. MUST begin one blank line after the description
2. MUST start with a single line in this format: `Why: <reason>`
3. MUST explain the motivation, user impact, risk reduction, or problem being solved
4. SHOULD be specific enough that a future reader can understand why the change exists without reading the full diff
5. SHOULD NOT merely repeat the description
6. MAY include additional paragraphs after a blank line
7. SHOULD explain why, not implementation details, unless implementation detail is needed for context

Use this standard pattern:

```
<type>[optional scope]: <description>

Why: <reason for the change>

[optional extra context paragraph]
```

## Footers (Optional)

Footers MAY be provided one blank line after the body:

1. Each footer MUST be: `<token>: <value>` or `<token> #<value>`
2. Token MUST use `-` instead of spaces (except `BREAKING CHANGE`)
3. Common footers: `Reviewed-by`, `Refs`, `Closes`, `Co-authored-by`

## Worked Examples

### Good Fit

> "Use `/conventional-commit` for the finished BD-142 work."

Expected outcome:

- diff checked for logical scope
- one small working commit
- body starts with `Why:`

### Poor Fit

> "Commit whatever is currently in the tree even though tests are failing."

Better path:

- finish verification first
- or split and stabilize a smaller working slice before committing

## Examples by Scenario

### Simple fix
```
fix: handle null pointer in user validation

Why: Prevent user creation from failing on profiles missing optional fields.
```

### Feature with scope
```
feat(dashboard): add export to CSV functionality

Why: Let operators download report data for offline analysis and compliance requests.
```

### Breaking change with `!`
```
refactor!: rename all public API methods

Why: Align the public API with the new SDK naming scheme before the next release.
```

### Multiple footers
```
feat: add user preferences sync

Why: Preserve a consistent experience for users switching between devices.

Reviewed-by: Alice Smith
Closes: #789
```

## Workflow

When creating a commit:

1. **Analyze changes** - Review staged files and diff to understand what changed
2. **Check commit size** - Confirm the staged changes are a single small, coherent, working unit; if not, split them before committing
3. **Determine type** - Choose the most appropriate type from the required types
4. **Determine scope** - If changes are localized, add a scope
5. **Write description** - Short, imperative, no period
6. **Write the why line** - Add a required `Why: ...` body line describing the motivation
7. **Check for breaking changes** - Add `!` or footer if API-breaking
8. **Add extra context/footer** - Add more body paragraphs or footers if useful

If a task reasonably produces multiple independent working checkpoints, repeat this workflow for each checkpoint rather than waiting to create one final aggregate commit.

## Commit Command

After constructing the message, use:

```bash
git commit -m "<type>[scope]: <description>" -m "Why: <reason>" [-m "<extra context>"] [-m "<footer>"]
```

Or for multi-line messages:

```bash
git commit -F - <<EOF
<type>[optional scope]: <description>

Why: <reason>

[optional extra context]

[optional footer(s)]
EOF
```
