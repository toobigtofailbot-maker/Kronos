# Kronos Project Status

Status: onboarding / planning baseline.

## Current Anchor

- Kronos source truth: `toobigtofailbot-maker/Kronos`.
- Reference-only repo: `crypto-perp-wedge`.
- Architecture stance: Kronos-centered, multi-asset, forecast-artifact-centered, custom-runner / hybrid, execution much later.
- Runtime status: UNKNOWN.
- Next intended package: KRONOS-002 Python 3.10/3.11 runtime audit and reproducibility proof.
- First empirical lean: crypto BTC/ETH 1h.
- NQQ unresolved: may mean QQQ, NQ, MNQ, or Nasdaq-100 exposure generally.

## What Is Known

- The repo contains Kronos model, tokenizer, predictor, examples, regression tests, fine-tuning material, and a web UI.
- README describes Kronos as a financial candlestick foundation model with Hugging Face model loading examples.
- `KronosPredictor` returns forecast-shaped OHLCV/amount dataframes from historical K-line inputs.
- These repo facts do not prove local runtime success, determinism, checkpoint availability, latency, memory, CPU behavior, or GPU behavior.

## What Is Unknown

Kronos runtime status is UNKNOWN.

The previous Python 3.13 audit failed before meaningful runtime evidence. Python 3.10/3.11 audit is required next.

Do not imply that Kronos installs successfully, tests pass, Hugging Face checkpoints are accessible, forecasting works, determinism is proven, latency/memory are known, or CPU/GPU behavior is known until KRONOS-002 or a later approved runtime package produces evidence.

Open user decisions include NQQ interpretation, first empirical asset class/timeframe, hardware/GPU availability, data budget, future brokers/exchanges, and artifact/tooling choices.

## Forbidden Current Work

Do not perform execution, risk automation, strategy optimization, data ingestion, fine-tuning, model refactors, tokenizer refactors, dependency changes, runtime audit, Hugging Face downloads, model inference, forecast artifact creation, portfolio logic, or OMS/execution work in KRONOS-001.

Kronos output is forecast input. Kronos output is not order authority, risk authority, portfolio authority, or strategy-promotion authority.

## Read Next

1. [Onboarding](onboarding/START_HERE.md)
2. [Long-term plan](project_plan/KRONOS_LONG_TERM_PLAN.md)
3. [System architecture](architecture/KRONOS_SYSTEM_ARCHITECTURE.md)
4. [Decision log](architecture/DECISION_LOG.md)
5. [Forbidden scope](boundaries/FORBIDDEN_SCOPE.md)
6. [Codex operating model](operations/CODEX_OPERATING_MODEL.md)
7. [WP drafting guide](operations/WP_DRAFTING_GUIDE.md)
8. [WP review guide](operations/WP_REVIEW_GUIDE.md)
9. [Token and context hygiene](operations/TOKEN_CONTEXT_HYGIENE.md)
