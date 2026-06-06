# Token and Context Hygiene

Token savings must not weaken repo inspection, validation, or boundary checks.

## Rules

- Read only relevant files.
- Summarize validation logs.
- Use concise `project_status` entries.
- Avoid repeating full onboarding text in every WP.
- Avoid pasting huge unrelated docs into Codex prompts.
- Link to repo docs instead of restating them where possible.
- Keep report-backs compact.
- Include exact changed files.
- Include exact validation results, not full logs unless failure occurs.
- Preserve safety and validation even when optimizing tokens.

## Practical Pattern

Use `docs/onboarding/START_HERE.md` and `docs/project_status.md` as onboarding anchors, then reference the specific architecture, boundary, or operations doc needed for the task.

For validation, include command names and pass/fail summaries. Include full logs only when a failure needs diagnosis.

## Future Options Only

These ideas are future options only, not implemented by KRONOS-001, not required for normal execution, and not a replacement for repo inspection or validation:

- diff summary helper
- compressed validation output helper
- code index / blast-radius helper
- session memory / decision memory
- RTK-style output compression
