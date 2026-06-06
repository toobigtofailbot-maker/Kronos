# Kronos Long-Term Plan

The accepted long-term plan is Kronos-centered, multi-asset, forecast-artifact-centered, custom-runner / hybrid, and execution much later.

## Phases

### Phase 0: repo/runtime proof

- Objective: prove the local repo can be installed and exercised under approved Python versions without changing protected runtime boundaries.
- Evidence produced: environment record, install evidence, minimal command evidence, regression or smoke output when approved.
- Gates: Python 3.10/3.11 audit succeeds, runtime claims are reproducible, no dependency drift is hidden.
- Still forbidden: execution, risk, OMS, strategy promotion, model/tokenizer refactors, data adapters beyond audit scope.

### Phase 1: Kronos deterministic inference wrapper

- Objective: define and then implement a narrow deterministic wrapper around existing Kronos predictor behavior if runtime proof passes.
- Evidence produced: deterministic settings, wrapper contract, minimal tests, repeatability evidence.
- Gates: runtime proof exists, inputs/outputs are explicit, nondeterminism is controlled or documented.
- Still forbidden: forecast-to-order logic, risk sizing, execution integration, broad model redesign.

### Phase 2: multi-asset data contracts + validators

- Objective: define asset/timeframe-neutral market data contracts and validators.
- Evidence produced: schemas, validation rules, rejected-record examples, fixture-level checks.
- Gates: contract handles first empirical market without locking crypto-only scope.
- Still forbidden: full ingestion engine, live feeds, broker/exchange credentials, execution paths.

### Phase 3: forecast artifact layer

- Objective: define durable forecast artifacts as the boundary between Kronos inference and downstream research.
- Evidence produced: artifact schema, lineage fields, validator, example synthetic or test fixture when approved.
- Gates: artifacts contain enough provenance to replay and audit.
- Still forbidden: forecast-to-order authority, strategy promotion, risk changes, live artifacts.

### Phase 4: evaluation/baseline runner

- Objective: compare Kronos forecasts against simple baselines and sanity checks.
- Evidence produced: baseline protocol, metrics, replay rules, evaluation outputs.
- Gates: simple baselines are present and Kronos value is measured rather than assumed.
- Still forbidden: production strategy optimization, paper/live trading, risk automation.

### Phase 5: probabilistic forecast/cone engine

- Objective: evaluate uncertainty and forecast cones from approved forecast artifacts.
- Evidence produced: cone design, calibration checks, coverage metrics.
- Gates: deterministic artifact lineage and evaluation protocol already exist.
- Still forbidden: using cones as direct sizing, risk, or execution authority.

### Phase 6: signal/alpha interpretation layer

- Objective: translate forecasts into research signals for evaluation only.
- Evidence produced: signal artifacts, indicator/baseline comparisons, interpretability notes.
- Gates: no signal can bypass artifact, evaluation, or risk simulation boundaries.
- Still forbidden: live strategy promotion, direct order routing, hardcoded risk policy.

### Phase 7: portfolio/risk simulation

- Objective: simulate risk and portfolio behavior from research artifacts.
- Evidence produced: simulation design, scenario outputs, drawdown and exposure analysis.
- Gates: simulation is separate from live risk authority and execution.
- Still forbidden: automated risk controls, OMS, live allocation, broker integration.

### Phase 8: paper/live architecture design

- Objective: design paper/live architecture only after research evidence justifies it.
- Evidence produced: ADRs, threat model, operational boundaries, manual controls.
- Gates: explicit approval to design beyond research layer.
- Still forbidden: implementation of live paths, credentials, execution router, reconciliation.

### Phase 9: execution router implementation much later

- Objective: implement execution only after separate approval, architecture, and safety gates.
- Evidence produced: execution design, OMS/reconciliation controls, risk caps, audit logs, dry-run evidence.
- Gates: explicit execution package approval and protected-boundary review.
- Still forbidden: any direct model-to-order path.

## First Package Roadmap Summary

These entries are roadmap anchors only, not drafted work packages.

- KRONOS-001 project knowledge lock
- KRONOS-002 Python 3.10/3.11 runtime audit and reproducibility proof
- KRONOS-003 environment/runtime guidance update based on audit
- KRONOS-004 multi-asset data contract design
- KRONOS-005 first empirical market/timeframe data contract
- KRONOS-006 forecast artifact design
- KRONOS-007 evaluation/baseline protocol
- KRONOS-008 deterministic inference wrapper design
- KRONOS-009 minimal deterministic inference wrapper if audit passes
- KRONOS-010 forecast artifact writer/validator
- KRONOS-011 minimal first-market data adapter
- KRONOS-012 first forecast artifact generation smoke test
- KRONOS-013 baseline runner skeleton
- KRONOS-014 forecast replay evaluator
- KRONOS-015 tooling ADR for vectorbt/DVC/MLflow/etc.
- KRONOS-016 probability cone design
- KRONOS-017 signal artifact design
- KRONOS-018 portfolio/risk simulation design
