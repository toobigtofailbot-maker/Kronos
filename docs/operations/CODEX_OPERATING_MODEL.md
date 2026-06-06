# Codex Operating Model

## Repo-First Rule

Repo truth wins over memory, assumptions, chat summaries, and older handoffs. Always inspect current files and diffs before making repo claims.

Source-truth hierarchy:

1. current Kronos repo truth
2. active WP scope
3. durable repo docs
4. external sources / reference repos
5. chat memory

`crypto-perp-wedge` is process reference only, not Kronos truth.

## Preflight Expectations

Before editing, check branch, remotes, working-tree state, recent history, existing docs, and overlap with the active WP. Stop if the tree is unexpectedly dirty, equivalent docs already exist, or the package would require forbidden paths.

## Branch and PR Expectations

Use narrow branches and mergeable work packages. Keep commits scoped to the WP. Do not merge without explicit instruction. PR descriptions should summarize changed files, validation evidence, and boundary checks.

## Package Types

- Docs-only packages create or update durable docs and do not run runtime audits unless explicitly approved.
- Runtime packages gather environment and execution evidence without changing project direction or protected execution boundaries.
- Implementation packages change code only inside explicit scope and must include package-appropriate validation.

## Scope Control

Move efficiently. Keep WPs narrow. Avoid huge prompts where repo docs can be referenced. Use compact report-backs. Summarize logs unless failure occurs.

Never skip validation or boundary checks to save tokens. Never touch protected or forbidden scope for speed.

No repo mutation outside WP scope. Every WP that changes project direction must update the relevant durable docs.

## Forbidden-Scope Check

Before and after edits, verify that protected runtime, OMS, reconciliation, risk caps, bounded overlay semantics, safe mode, configs, live-trading paths, model/tokenizer code, dependency files, data paths, tests, examples, and credentials are untouched unless explicitly authorized.

## Validation and Report-Back

Validation must match package type. Docs-only packages should run formatting/diff checks and forbidden-path checks, not heavy runtime tests by default. Report exact changed files and exact validation results.

## Knowledge-Lock Policy

Important project knowledge belongs in durable repo docs, not only in chat memory. Future chats should onboard from `docs/onboarding/START_HERE.md` and `docs/project_status.md` before relying on summaries.
