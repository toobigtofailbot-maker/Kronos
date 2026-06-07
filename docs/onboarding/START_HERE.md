# Start Here

This is the first file future chats and Codex sessions should read for the Kronos side project.

## Source Truth

- Primary source truth: `toobigtofailbot-maker/Kronos`.
- `crypto-perp-wedge`: process/reference lessons only.
- Chat memory: not source truth.

Source-truth hierarchy:

1. Current Kronos repo truth
2. Active WP scope
3. Durable repo docs
4. External sources / reference repos
5. Chat memory

Repo truth wins over memory, handoffs, summaries, and assumptions.

## What This Project Is

This project is Kronos-centered, multi-asset over time, forecast-artifact-centered, custom-runner / hybrid, and validation-first. The long-term system direction is to use Kronos forecasts as research inputs that can be evaluated, replayed, compared against baselines, and later consumed by separate signal, risk, portfolio, and execution layers.

Kronos is central because this repo contains the Kronos model, tokenizer, predictor, examples, regression tests, fine-tuning material, and web UI. Those facts make Kronos the starting point for this side project, but they do not prove local runtime status.

The system is multi-asset opportunity discovery over time. The near-term proof path is BTCUSDT and ETHUSDT Binance USDT-M perpetual trade-price klines at 1h, but the project must preserve future support for equities/ETFs, Nasdaq-100 exposure, index futures, FX, and other liquid markets where justified.

## What This Project Is Not

This project is not BTC-only, crypto-only, generic-framework-first, a live or paper trading project yet, an execution-router project yet, or a model-to-order system.

Kronos output is forecast input. Kronos output is not order authority, risk authority, portfolio authority, or strategy-promotion authority.

## Runtime Status

Kronos runtime status is PARTIAL.

KRONOS-002 produced Python 3.11 CPU evidence: dependency install succeeded, targeted pinned regression tests passed, Hugging Face checkpoints were public/ungated, minimal inference ran, and fixed-seed CPU repeatability matched exactly. Remaining blockers: full `pytest -q` collects `finetune/qlib_test.py` and fails without `qlib`; README example data is missing; GPU/MPS behavior is unavailable and untested.

Use [runtime environment guidance](../runtime/ENVIRONMENT.md) and the [runtime checklist](../runtime/RUNTIME_CHECKLIST.md) before any future runtime work.

## Next Read Order

1. [Project status](../project_status.md)
2. [Long-term plan](../project_plan/KRONOS_LONG_TERM_PLAN.md)
3. [System architecture](../architecture/KRONOS_SYSTEM_ARCHITECTURE.md)
4. [Decision log](../architecture/DECISION_LOG.md)
5. [Forbidden scope](../boundaries/FORBIDDEN_SCOPE.md)
6. [Codex operating model](../operations/CODEX_OPERATING_MODEL.md)
7. [Multi-asset data contract](../data/MULTI_ASSET_DATA_CONTRACT.md)
8. [Kronos input contract](../data/KRONOS_INPUT_CONTRACT.md)
9. [Crypto 1h first empirical target](../data/CRYPTO_1H_FIRST_TARGET.md)
10. [Runtime environment guidance](../runtime/ENVIRONMENT.md)
11. [Runtime checklist](../runtime/RUNTIME_CHECKLIST.md)
12. [WP drafting guide](../operations/WP_DRAFTING_GUIDE.md)
13. [WP review guide](../operations/WP_REVIEW_GUIDE.md)
14. [Token and context hygiene](../operations/TOKEN_CONTEXT_HYGIENE.md)
