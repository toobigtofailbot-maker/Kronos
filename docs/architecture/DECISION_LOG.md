# Decision Log

This file records accepted, open, deferred, rejected/forbidden, and future-option decisions for the Kronos side project.

## Accepted Decisions

- Kronos-centered direction accepted.
- Multi-asset direction accepted.
- Forecast artifact boundary accepted.
- Custom-runner / hybrid direction accepted.
- Execution is much later.
- `crypto-perp-wedge` is process reference only.
- Simple baselines are required to measure Kronos value.
- Classic indicators are not banned.
- Architecture stance is Kronos forecast-artifact core + custom multi-asset data/research/risk layers + hybrid tooling where useful + execution routers much later.
- KRONOS-002 runtime status is PARTIAL based on Python 3.11 CPU evidence: dependency install succeeded, targeted pinned regression tests passed, Hugging Face checkpoints were public/ungated, minimal inference ran, and fixed-seed CPU repeatability matched exactly.

Classic indicators remain useful as baselines, filters, risk features, sanity checks, regime descriptors, interpretable confluence, and fallback comparisons.

## Current Runtime Status

- Runtime status PARTIAL.
- KRONOS-002 audit: Python 3.11 CPU path works for install, targeted regression, Hugging Face access, minimal inference, and exact fixed-seed repeatability.
- Remaining runtime blockers: full `pytest -q` fails collecting `finetune/qlib_test.py` without `qlib`; README example data is missing; GPU/MPS behavior is unavailable and untested.

## Current Empirical Lean

- First empirical lean: crypto BTC/ETH 1h.
- This is not a permanent crypto-only decision.

## Open Decisions

- NQQ unresolved: may mean QQQ, NQ, MNQ, or Nasdaq-100 exposure generally.
- First empirical asset class.
- First empirical timeframe.
- Hardware / GPU availability.
- Data budget.
- Future brokers/exchanges.
- Artifact tooling choice.
- vectorbt/Qlib/Freqtrade/Nautilus support-tooling decisions.
- Whether `qlib` test collection should be optional, separately documented, or supported by a development dependency path.
- Whether README example data should be restored, externally documented, or example behavior updated in a later package.

## Deferred Decisions

- fine-tuning deferred
- execution deferred
- portfolio/risk simulation deferred
- multi-market ingestion engine deferred
- Kronos inference service deferred
- paper/live trading deferred
- GPU/CUDA runtime audit deferred until suitable hardware is available.

## Candidate Parameters Not Locked

- 1% risk per trade not locked
- Kelly Criterion not locked
- -3% daily drawdown kill switch not locked
- 30-50 Monte Carlo samples per asset not locked

## Rejected / Forbidden Decisions

- BTC-only direction is rejected.
- Crypto-only direction is rejected.
- Generic framework-first replacement is rejected.
- Direct forecast-to-order logic is forbidden.
- Direct model-to-risk changes are forbidden.
- Hardcoded risk policy is forbidden.
- Hardcoded Kelly sizing is forbidden.
- Hardcoded daily drawdown threshold is forbidden.
- Execution, OMS, reconciliation, broker/exchange API integration, live WebSocket feeds, and credential handling are forbidden for current work.

## Future Options

- vectorbt, Qlib, Freqtrade, Nautilus, DVC, MLflow, or similar tooling may be evaluated later as support tools.
- A Kronos inference service may be designed later if local runtime, reproducibility, and artifact boundaries are proven.
- Paper/live architecture may be designed later after research evidence and explicit approval.
