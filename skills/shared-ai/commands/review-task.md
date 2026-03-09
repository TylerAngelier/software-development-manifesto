# `/review-task`

Use this runbook when the user wants a fresh-context review of one task, patch, or diff.

## Goal

Review the work against the task contract and verification evidence without turning the review into a rewrite session.

## Inputs

- task ID or equivalent scope label
- linked spec path when available
- changed files or diff summary
- verification output

## Output

- prioritized findings
- missing tests or weak verification notes
- scope drift notes
- explicit `no findings` when clean

## Workflow

1. Read the task description or bug summary.
2. Read the linked spec if one exists.
3. Read the changed files or diff summary.
4. Read verification output and note gaps.
5. Review for:
   - correctness against the spec or bug intent
   - regressions and edge cases
   - security and data handling issues
   - insufficient tests or weak manual verification
   - scope drift
6. Return findings in priority order, or explicitly say `no findings`.

## Review Rules

- Prefer concrete bugs and risks over style comments.
- Treat weak or missing verification as a real finding when it affects confidence.
- Do not propose broad rewrites when a bounded fix exists.
- Stay read-only unless the controller explicitly changes roles.
