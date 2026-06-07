# Kronos Project Status

Status: onboarding / planning baseline.

## Current Anchor

- Kronos source truth: `toobigtofailbot-maker/Kronos`.
- Reference-only repo: `crypto-perp-wedge`.
- Architecture stance: Kronos-centered, multi-asset, forecast-artifact-centered, custom-runner / hybrid, execution much later.
- Runtime status: PARTIAL.
- Runtime reason: Python 3.11 install, pinned CPU regression tests, Hugging Face access, minimal inference, and exact fixed-seed CPU repeatability passed; full pytest collection, README example data, and GPU/MPS behavior remain blocked or untested.
- Runtime audit: [KRONOS runtime audit](runtime/KRONOS_RUNTIME_AUDIT.md).
- Runtime guidance: [environment guidance](runtime/ENVIRONMENT.md) and [runtime checklist](runtime/RUNTIME_CHECKLIST.md).
- Data contracts: [multi-asset data contract](data/MULTI_ASSET_DATA_CONTRACT.md) and [Kronos input contract](data/KRONOS_INPUT_CONTRACT.md).
- Next intended package: KRONOS-005 first empirical market/timeframe data contract.
- First empirical lean: crypto BTC/ETH 1h.
- NQQ unresolved: may mean QQQ, NQ, MNQ, or Nasdaq-100 exposure generally.

## What Is Known

- The repo contains Kronos model, tokenizer, predictor, examples, regression tests, fine-tuning material, and a web UI.
- README describes Kronos as a financial candlestick foundation model with Hugging Face model loading examples.
- `KronosPredictor` returns forecast-shaped OHLCV/amount dataframes from historical K-line inputs.
- These repo facts do not prove local runtime success, determinism, checkpoint availability, latency, memory, CPU behavior, or GPU behavior.

## What Is Unknown

Kronos runtime status is PARTIAL.

KRONOS-002 produced Python 3.11 CPU evidence: dependency install succeeded, targeted pinned regression tests passed, Hugging Face checkpoints were public/ungated, minimal inference ran, and fixed-seed CPU repeatability matched exactly. Remaining blockers: full `pytest -q` collects `finetune/qlib_test.py` and fails without `qlib`; README example data is missing; GPU/MPS behavior is unavailable and untested.

Open user decisions include NQQ interpretation, first empirical asset class/timeframe, hardware/GPU availability, data budget, future brokers/exchanges, and artifact/tooling choices.

## Forbidden Current Work

Do not perform execution, risk automation, strategy optimization, data ingestion, fine-tuning, model refactors, tokenizer refactors, dependency changes, forecast artifact creation, portfolio logic, or OMS/execution work outside an approved package.

Kronos output is forecast input. Kronos output is not order authority, risk authority, portfolio authority, or strategy-promotion authority.

## Read Next

1. [Onboarding](onboarding/START_HERE.md)
2. [Long-term plan](project_plan/KRONOS_LONG_TERM_PLAN.md)
3. [System architecture](architecture/KRONOS_SYSTEM_ARCHITECTURE.md)
4. [Decision log](architecture/DECISION_LOG.md)
5. [Forbidden scope](boundaries/FORBIDDEN_SCOPE.md)
6. [Codex operating model](operations/CODEX_OPERATING_MODEL.md)
7. [Multi-asset data contract](data/MULTI_ASSET_DATA_CONTRACT.md)
8. [Kronos input contract](data/KRONOS_INPUT_CONTRACT.md)
9. [Runtime environment guidance](runtime/ENVIRONMENT.md)
10. [Runtime checklist](runtime/RUNTIME_CHECKLIST.md)
11. [WP drafting guide](operations/WP_DRAFTING_GUIDE.md)
12. [WP review guide](operations/WP_REVIEW_GUIDE.md)
13. [Token and context hygiene](operations/TOKEN_CONTEXT_HYGIENE.md)
