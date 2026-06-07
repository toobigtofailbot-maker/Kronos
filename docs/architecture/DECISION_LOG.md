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
- KRONOS-003 documents Python 3.11 as the current proven runtime-guidance path based on KRONOS-002, with targeted regression as the current proven test command.
- KRONOS-004 defines canonical multi-asset OHLCVA and Kronos input contracts as docs-only design.
- KRONOS-004 accepts Binance USDT-M trade-price klines as the provisional first crypto-perp OHLCVA mapping for future BTC/ETH 1h specialization.
- KRONOS-005 specializes the first empirical target as BTCUSDT and ETHUSDT Binance USDT-M perpetual trade-price klines at 1h, docs-only.
- The project's long-term goal is multi-asset opportunity discovery / market opportunity scanning, not crypto-only.
- The KRONOS-005 first empirical target does not limit long-term architecture.
- NQQ was a spelling mistake and means general Nasdaq-100 exposure for now; QQQ, NQ, MNQ, and other Nasdaq-100 exposures remain possible future instruments.
- Current local hardware includes Windows x64, Intel Core Ultra 7 155H CPU, 32 GB RAM, approximately 2 TB NVMe storage, NVIDIA RTX 4050 Laptop GPU with 6 GB VRAM, and Intel Arc iGPU.
- CPU inference path is proven by KRONOS-002; local NVIDIA GPU remains unvalidated; Apple MPS is not applicable on this machine.
- Data-source policy starts free / low-cost; no paid-feed dependency is accepted for the first empirical proof.
- Future broker/exchange account options may include Binance, Alpaca, and IBKR, but they are not execution authorization.
- Zero-shot forecast work comes first; fine-tuning remains deferred.
- Paper/live trading remains distant.
- `vectorbt` may be evaluated later as support tooling, not as the architecture center.
- Qlib remains reference-only/deferred and must not be added as a core dependency now.
- Freqtrade and Nautilus are deferred until forecast artifacts, evaluation, and baseline evidence exist.
- Artifact tracking starts lightweight; DVC, MLflow, Pandera, Great Expectations, or similar tools are later evaluation candidates.

Classic indicators remain useful as baselines, filters, risk features, sanity checks, regime descriptors, interpretable confluence, and fallback comparisons.

## Current Runtime Status

- Runtime status PARTIAL.
- KRONOS-002 audit: Python 3.11 CPU path works for install, targeted regression, Hugging Face access, minimal inference, and exact fixed-seed repeatability.
- Remaining runtime blockers: full `pytest -q` fails collecting `finetune/qlib_test.py` without `qlib`; README example data is missing; GPU/MPS behavior is unavailable and untested.

## Current Empirical Lean

- First empirical target: BTCUSDT and ETHUSDT Binance USDT-M perpetual trade-price klines at 1h.
- This is not a permanent crypto-only decision.
- Long-term timeframe ambition is roughly 5m to 1d, sequenced later.

## Open Decisions

- Exact Nasdaq-100 instrument remains unresolved: QQQ ETF, NQ futures, MNQ futures, or other Nasdaq-100 exposure.
- Data acquisition method for the first empirical target.
- Raw data file format.
- Normalized storage format.
- Gap manifest format.
- Non-core feature stream contracts.
- GPU/CUDA readiness remains unvalidated.
- Paid data vendors and exact budget remain future gated decisions.
- Artifact tooling choice.
- vectorbt/Qlib/Freqtrade/Nautilus support-tooling decisions.
- Whether `qlib` test collection should be optional, separately documented, or supported by a development dependency path.
- Whether README example data should be restored, externally documented, or example behavior updated in a later package.
- pytest/dev dependency policy remains open.
- Example data/example path behavior remains open.
- Equities/ETFs, futures, and FX vendor-specific mappings remain deferred.
- Non-core feature stream contracts remain deferred.
- Validator implementation remains deferred.
- Whether `amount` can be nullable for asset classes without official turnover remains open.

## Deferred Decisions

- fine-tuning deferred
- execution deferred
- broker/exchange/API integration deferred
- portfolio/risk simulation deferred
- multi-market ingestion engine deferred
- data adapter implementation deferred
- validator implementation deferred
- Kronos inference service deferred
- paper/live trading deferred
- GPU/CUDA runtime audit deferred until suitable hardware is available.
- GPU/MPS audit remains deferred.
- paid data vendor and budget decisions deferred
- heavier artifact/validation tools deferred

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
