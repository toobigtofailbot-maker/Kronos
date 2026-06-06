# WP Drafting Guide

Future WPs must be generated from repo truth, not chat memory. Packages must stay narrow, forbidden scope must be explicit, validation must be package-appropriate, stop conditions must be included, and report-backs must be compact and evidence-based.

Do not import package IDs, artifact IDs, strategy decisions, status, current packages, or architecture decisions from `crypto-perp-wedge`.

## Compact WP Template

```text
Package identity
- Package label:
- Package title:
- Suggested branch:
- Effective base:
- Package class:

Objective
- What this package must accomplish:
- What this package must not accomplish:

Why now
- Reason this package is next:

Hard dependencies
- Required repo state:
- Required prior evidence:

Preflight
- Commands to run before editing:
- Stop criteria from preflight:

Repo-truth discovery
- Files to read:
- Questions to answer from current repo:

Files to create
- Exact paths:

Files to modify
- Exact paths:
- Allowed edit limits:

Files to read only
- Exact paths or directories:

Files forbidden to modify
- Exact paths and path patterns:

Implementation/documentation requirements
- Required behavior or content:
- Required non-claims:

Validation commands
- Package-appropriate commands:
- Forbidden-path checks:

Stop conditions
- Conditions that require stopping and reporting:

Commit/PR instructions
- Branch:
- Commit message:
- PR title/body:

Report-back format
- Branch/base:
- Changed files:
- Validation:
- Boundary check:
- PR status:
```

## Drafting Rules

- Start from current Kronos repo truth.
- Keep each WP narrow enough to review and merge.
- Make forbidden scope explicit.
- Separate files to create, modify, read only, and never touch.
- Include package-appropriate validation commands.
- Include stop conditions.
- Require compact report-backs with changed files and validation evidence.
- Do not let research/workbench artifacts gain execution authority unless a later approved path promotes them.
