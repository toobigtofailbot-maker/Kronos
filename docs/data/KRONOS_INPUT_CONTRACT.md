# Kronos Input Contract

## A. Scope

This document defines the data shape expected before Kronos inference. It derives the Kronos-specific input view from current repo truth and the canonical multi-asset data contract.

It does not implement a wrapper, adapter, validator, artifact writer, inference path, data source, or runtime path.

## B. Kronos Repo Truth

Current repo truth:

- `KronosPredictor` defines price columns as `open`, `high`, `low`, and `close`.
- Predictor input requires a pandas DataFrame containing those four price columns.
- `volume` and `amount` are handled by repo code but must be explicitly governed by this project contract.
- If `volume` is missing, repo predictor behavior fills both `volume` and `amount` with zero.
- If `volume` exists and `amount` is missing, repo predictor behavior derives `amount` as `volume` multiplied by the row mean of `open`, `high`, `low`, and `close`.
- The predictor rejects NaN values in the six model-view columns after any fill/derive behavior.
- Time features are derived from `x_timestamp` and `y_timestamp` using minute, hour, weekday, day, and month fields.
- Predictor output is a forecast-shaped DataFrame with `open`, `high`, `low`, `close`, `volume`, and `amount`, indexed by the supplied `y_timestamp`.

The README states that `df` must include `open`, `high`, `low`, and `close`, while `volume` and `amount` are optional. The README and regression tests use `x_timestamp` for historical rows and `y_timestamp` for future forecast rows.

Runtime status remains PARTIAL per current durable docs.

## C. Required Input Columns

Required model-view columns:

| Column | Requirement |
| --- | --- |
| `open` | Numeric opening price from the selected core price stream. |
| `high` | Numeric high price from the selected core price stream. |
| `low` | Numeric low price from the selected core price stream. |
| `close` | Numeric closing price from the selected core price stream. |

The project contract prefers providing all six columns for future research consistency:

- `open`
- `high`
- `low`
- `close`
- `volume`
- `amount`

## D. Optional / Derived Columns

No extra columns should be passed to Kronos core OHLCVA unless the repo predictor explicitly supports them.

Non-core fields belong in separate feature streams or metadata. Funding, mark price, index price, premium, open interest, taker flow, trade count, corporate actions, roll metadata, FX spread/rollover fields, and calendar/session features must not be overloaded into `volume` or `amount`.

## E. Timestamp Inputs

Kronos input requires timestamp sequences separate from the OHLCVA DataFrame:

| Timestamp input | Requirement |
| --- | --- |
| `x_timestamp` | Historical timestamps aligned one-to-one with input rows. |
| `y_timestamp` | Mechanically generated future timestamps for the forecast horizon. |

Both must be timezone-aware or explicitly UTC-normalized before use. No `y_timestamp` may be inferred from actual future observed bars. Future timestamps must be generated from the deterministic contract schedule for the selected asset, venue, timeframe, and session model.

## F. Output Expectations

Kronos forecast output is forecast data, not a signal and not an order. The predictor returns forecast-shaped `open`, `high`, `low`, `close`, `volume`, and `amount` values indexed by `y_timestamp`.

Output needs a future forecast artifact schema before downstream research, signal, evaluation, risk, portfolio, or execution layers can consume it. Kronos output is not order authority, risk authority, portfolio authority, or strategy-promotion authority.

## G. Missing Volume / Amount Policy

For the first empirical crypto-perp target, `volume` and `amount` are required from source data.

Do not rely on silent zero-fill or synthetic `amount` unless a later package explicitly approves it. If source `amount` is missing, reject by default. If source `volume` is missing, reject by default.

Repo behavior may tolerate missing fields by filling or deriving them, but the project contract is stricter for research integrity and provenance.

## H. Normalization and Leakage Notes

Do not use future observed prices to construct input rows. Do not repair missing candles using future data. Do not allow `y_timestamp` construction to depend on observed future market data.

Calendar/session construction must be deterministic from the contract. Normalization must be fit only from permitted historical input context for the forecast being generated.

## I. Accepted First Input View

First empirical candidate for KRONOS-005:

| Field | Candidate value |
| --- | --- |
| Symbols | BTCUSDT and ETHUSDT |
| Venue/source family | Binance USD-M Futures |
| Instrument | USDT-M perpetual |
| Price stream | Trade-price klines |
| Timeframe | 1h |
| Columns | `open`, `high`, `low`, `close`, base asset volume mapped to `volume`, quote asset volume mapped to `amount` |
| Timestamps | UTC open/close time |

This is a lean for KRONOS-005, not an implementation in KRONOS-004.

## J. Rejected Input Conditions

Reject input when any of these conditions are present:

- missing required columns
- extra core-field overloading
- non-UTC timestamps after normalization
- duplicate timestamps
- non-monotonic timestamps
- unexpected gaps
- NaN or infinity values
- OHLC inconsistency
- negative `volume` or `amount`
- unknown `amount` semantics
- mark/index/funding/open-interest fields stuffed into OHLCVA
- taker flow or trade count stuffed into `volume` or `amount`
- future leakage in repairs or features
- `y_timestamp` constructed from actual future observed bars

## K. Future Validator Requirements

Future Kronos input validators should implement the validator requirements in [Multi-Asset Data Contract](MULTI_ASSET_DATA_CONTRACT.md), including schema, numeric, timestamp, monotonicity, interval/session, duplicate, gap, OHLC, volume/amount, provenance, feature publication-time, and no-future-data checks.

KRONOS-004 does not create validator code or executable schemas.

## L. Open Questions

- Confirm first empirical asset class, symbols, and timeframe.
- Resolve NQQ before any Nasdaq-100 exposure contract assumes QQQ, NQ, MNQ, or a broader exposure.
- Decide whether future model-view contracts should ever permit missing `amount` for asset classes where official turnover is unavailable.
- Decide whether mark, index, funding, open interest, taker flow, or trade count should become separate feature streams.
- Decide future validator implementation format in a later package.
- Decide future forecast artifact schema before treating predictor output as durable research output.
