# WP Review Guide

Review Codex outputs as diffs against the active WP and current repo truth.

## Compact Review Template

```text
Decision: merge-ready / follow-up needed / blocked

Scope check
- Did the output stay inside the package?

Changed-file check
- Which files changed?
- Do changed files match the WP?

Semantic check
- Do docs/code state the right project facts?
- Are runtime claims evidence-backed?

Validation check
- Which commands ran?
- Did any fail?
- Are skipped checks justified?

Boundary check
- Were forbidden files untouched?
- Was any model/strategy/risk/execution authority introduced?

Open issues
- Remaining blockers or follow-up items:

Next action
- Merge, request follow-up, block, or draft a new WP:
```

## Required Checks

- Changed files match package scope.
- Forbidden files are untouched.
- Runtime claims are evidence-backed.
- Docs update project knowledge correctly.
- No model, strategy, risk, portfolio, OMS, reconciliation, execution, or forecast-to-order authority is introduced.
- Validation evidence is adequate for the package type.
- README changes, if any, are limited to an explicitly justified tiny pointer.

## Follow-Up Prompt Pattern

If follow-up is needed, always provide a copy-paste-ready Codex follow-up prompt:

```text
You are in C:\GitHub\Kronos on branch <branch>.

Goal:
<specific correction>

Current evidence:
<changed files, validation results, issue found>

Scope:
<allowed files and forbidden files>

Required validation:
<commands>

Stop if:
<stop conditions>

Report back:
<compact format>
```
