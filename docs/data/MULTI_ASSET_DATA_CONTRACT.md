# Multi-Asset Data Contract

## A. Scope

This document defines the design-only canonical multi-asset OHLCVA bar contract for future Kronos input preparation. It does not implement ingestion, validation code, data adapters, data fetching, schemas as executable artifacts, inference wrappers, forecast artifacts, or strategy/risk/portfolio/execution paths.

The contract exists so future packages can prepare market data without timestamp ambiguity, silent repair, crypto-only lock-in, BTC-only lock-in, or accidental use of non-core feature streams as OHLCVA fields.

## B. Source-Truth Hierarchy

1. Kronos repo input/output behavior
2. This data contract
3. Official exchange/vendor documentation
4. Future project data samples
5. Chat memory

Repo truth remains higher authority than this design if later current code or accepted docs conflict with it.

## C. Canonical Bar Identity

Each normalized bar must carry enough identity to make the market, source, instrument, timeframe, and schedule unambiguous.

| Field | Requirement |
| --- | --- |
| `asset_id` | Stable project-level identifier for the tradable or observed instrument. |
| `asset_class` | Asset class such as `crypto_perp`, `equity`, `etf`, `future`, or `fx`. Exact enum values are deferred. |
| `venue` | Venue or exchange family used for the data source. |
| `source` | Vendor/API/source system that supplied the raw record. |
| `symbol_native` | Native source symbol exactly as the source uses it. |
| `symbol_canonical` | Project-normalized symbol. Exact normalization rules are deferred. |
| `instrument_type` | Spot, perpetual, ETF share, listed future contract, FX pair, or other explicit type. |
| `market_type` | Market structure such as cash, perpetual, deliverable future, continuous future, or FX spot. |
| `timeframe` | Bar interval, for example `1h`. |
| `bar_open_time_utc` | UTC opening timestamp for the bar. |
| `bar_close_time_utc` | UTC closing timestamp for the bar. |
| `schema_version` | Contract version used by the normalized view. |

Exact enum values, symbol normalization rules, and versioning mechanics will be refined in later implementation packages.

## D. Canonical OHLCVA Fields

The canonical market fields are:

| Field | Requirement |
| --- | --- |
| `open` | Numeric opening price from the selected core price stream. |
| `high` | Numeric high price from the selected core price stream. |
| `low` | Numeric low price from the selected core price stream. |
| `close` | Numeric closing price from the selected core price stream. |
| `volume` | Numeric volume with explicit asset-class-specific semantics. |
| `amount` | Numeric turnover/notional/quote-volume field with explicit asset-class-specific semantics. |

`open`, `high`, `low`, and `close` must come from one selected core price stream. `volume` must have asset-class-specific semantics. `amount` must have asset-class-specific turnover or notional semantics.

Do not synthesize `amount` silently. Do not overload `amount` with unrelated fields. Do not stuff funding, mark price, index price, premium, open interest, trade count, taker flow, corporate actions, roll metadata, spreads, or calendar flags into core OHLCVA fields.

## E. Time and Timezone Policy

Canonical timestamps must be UTC. The normalized contract requires both `bar_open_time_utc` and `bar_close_time_utc`; local timezone storage is not canonical time.

Rules:

- Timestamps must be monotonic increasing within each asset/timeframe/source stream.
- Bar intervals must pass strict checks within the selected session model.
- Canonical timestamps must not depend on daylight-saving-time behavior.
- `bar_open_time_utc` and `bar_close_time_utc` must be explicit after normalization.
- Future `y_timestamp` values for Kronos must be constructed mechanically from a valid bar schedule, not from observed future data.
- Timezone-naive source timestamps must be rejected unless a later package defines a reproducible UTC normalization rule for that source.

## F. Session and Calendar Policy

Session rules are asset-class-specific:

| Asset class | Session/calendar policy |
| --- | --- |
| Crypto | 24/7 UTC continuous calendar, subject to source outages or explicitly documented exchange interruptions. |
| Equities / ETFs | Exchange sessions, holidays, and explicit regular-trading-hours versus extended-hours distinction. |
| Futures | Exchange sessions, maintenance breaks, contract expiries, and roll policy where continuous contracts are used. |
| FX | Broker/venue-specific quasi-24/5 calendar, rollover behavior, weekend gaps, and holiday/liquidity gaps. |

Do not treat market-closed periods as missing data. Do not silently stitch sessions if doing so changes economic meaning.

## G. Asset-Class Policies

| Asset class | Core price stream | `volume` meaning | `amount` meaning | Key complications | Status |
| --- | --- | --- | --- | --- | --- |
| Crypto perpetual | Trade-price bars for first target | Base asset volume for Binance USDT-M first mapping | Quote asset volume for Binance USDT-M first mapping | Mark/index/premium/funding/OI/taker-flow are separate streams; 24/7 gaps need outage handling | DEFINED FOR FIRST EMPIRICAL TARGET |
| Equities / ETFs | Vendor/exchange trade bars, adjusted or unadjusted policy required | Shares traded | Official dollar volume or explicitly defined turnover proxy | Corporate actions, symbol changes, delistings, RTH/ETH, adjustments | DESIGN REQUIREMENT |
| Futures | Contract trade bars or approved continuous series | Contracts traded | Notional turnover if official or explicitly computed | Contract identifiers, multipliers, rolls, settlement versus last-trade close, maintenance breaks | DESIGN REQUIREMENT |
| FX | Explicit bid, ask, mid, or trade stream | Vendor-defined tick volume or real volume if available | Vendor-defined notional policy if available | Decentralized market, broker source, rollover, weekend and holiday gaps | OPEN |

## H. Crypto / Binance USDT-M First Mapping

The provisional first mapping for KRONOS-005 is:

| Contract field | Binance USDT-M source field |
| --- | --- |
| `asset_class` | `crypto_perp` |
| `venue` | `binance_usdt_m` |
| `market_type` | `perpetual` |
| Core price stream | Trade-price kline/candlestick data |
| `open` / `high` / `low` / `close` | Trade-price kline OHLC |
| `volume` | Base asset volume from trade-price kline response |
| `amount` | Quote asset volume from trade-price kline response |
| `bar_open_time_utc` | Binance kline open time |
| `bar_close_time_utc` | Binance kline close time |

Official Binance USD-M Futures documentation states that trade-price klines are bars for a symbol, identified by open time, and the response includes open, high, low, close, volume, close time, quote asset volume, number of trades, taker-buy base volume, and taker-buy quote volume. The trade-price kline endpoint is therefore the default core OHLCVA source for first empirical BTC/ETH 1h tests, subject to KRONOS-005.

Official Binance documentation also describes separate mark-price klines, index-price klines, premium-index klines, funding-rate history, open interest, and taker buy/sell volume endpoints. These are not core OHLCVA fields unless a later feature contract designs them explicitly:

- funding rate
- funding time
- mark price
- index price
- premium index
- open interest
- taker buy base volume
- taker buy quote volume
- number of trades
- basis
- long/short ratios

These streams may become separate feature streams later, with explicit publication-time alignment.

Do not stuff funding, mark/index/premium, open interest, taker flow, or number-of-trades into `volume` or `amount`. Do not combine trade-price, mark-price, and index-price candles into one OHLCVA stream.

Official source references used for this section:

- Binance USD-M Futures Kline/Candlestick Data: <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Kline-Candlestick-Data>
- Binance USD-M Futures Mark Price Kline/Candlestick Data: <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Mark-Price-Kline-Candlestick-Data>
- Binance USD-M Futures Index Price Kline/Candlestick Data: <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Index-Price-Kline-Candlestick-Data>
- Binance USD-M Futures Premium Index Kline Data: <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Premium-Index-Kline-Data>
- Binance USD-M Futures Funding Rate History: <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Get-Funding-Rate-History>
- Binance USD-M Futures Open Interest: <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Open-Interest>
- Binance USD-M Futures Taker Buy/Sell Volume: <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Taker-BuySell-Volume>

## I. Equities / ETFs Policy

Equities and ETFs remain design-level only in KRONOS-004 because vendor-specific official docs were not inspected for this package.

Future equity/ETF contracts must define:

- adjusted versus unadjusted prices
- split and dividend adjustment policy
- regular-trading-hours versus extended-hours bars
- `volume` as shares traded
- `amount` as official dollar volume or an explicitly defined turnover proxy
- corporate-action handling
- symbol changes and delistings

NQQ remains unresolved and may mean QQQ ETF, NQ futures, MNQ futures, or Nasdaq-100 exposure generally. Do not write data contract decisions that assume one of these without user confirmation or a later package.

## J. Futures Policy

Futures remain design-level only in KRONOS-004 because venue/vendor-specific official docs were not inspected for this package.

Future futures contracts must define:

- contract identifier
- front-month versus continuous contract policy
- roll rule
- adjusted versus unadjusted continuous series
- contract multiplier
- `volume` as contracts traded
- `amount` as notional turnover if official or explicitly computed
- session and maintenance breaks
- settlement versus last-trade close policy

## K. FX Policy

FX remains design-level only in KRONOS-004 because broker/vendor-specific official docs were not inspected for this package.

Future FX contracts must define:

- bid, ask, or mid price selection
- broker/venue source
- tick volume versus real volume policy
- amount/notional policy
- rollover time
- weekend gaps
- holiday and liquidity gaps

FX volume and amount are especially vendor-dependent and must not be invented silently.

## L. Missing, Duplicate, and Gap Policy

Validation requirements:

- Duplicate timestamps are rejected.
- Non-monotonic timestamps are rejected.
- Missing bars are detected according to the applicable session calendar.
- NaN and infinity values are rejected.
- OHLC consistency is checked.
- `volume` and `amount` must be nonnegative unless a future asset-class exception is documented.
- Strict interval checks must use the selected session calendar.
- A gap manifest is required for any accepted gap.
- Silent interpolation is forbidden.
- Forward-filling OHLC prices into missing tradable bars is forbidden.

Market-closed intervals under an explicit session/calendar model are not missing bars.

## M. Reject-vs-Repair Policy

Default policy: reject invalid input.

Repairs must be explicit, logged, justified, and reproducible. No repair may be silent. No repair may use future data. No repair may alter raw-source files.

Allowed repair mechanics, if later approved, must produce a separate normalized output with provenance that records the repair policy and affected bars.

## N. Provenance and Lineage Fields

Future normalized views must carry provenance sufficient for replay and audit:

| Field | Requirement |
| --- | --- |
| `source_name` | Source system or vendor name. |
| `source_documentation_url` | Official documentation URL used for field semantics. |
| `source_downloaded_at_utc` or `observed_at_utc` | UTC time the source data was downloaded or observed. |
| `raw_source_path_or_reference` | Raw file path, object key, vendor reference, or source locator. |
| `raw_data_hash` | Hash of raw source data. Not implemented in KRONOS-004. |
| `normalized_data_hash` | Hash of normalized data. Not implemented in KRONOS-004. |
| `normalization_policy_version` | Version of the normalization policy. |
| `contract_version` | Data contract version used. |
| `quality_flags` | Explicit quality flags, not hidden implicit assumptions. |
| `known_gaps_reference` | Reference to accepted gap manifest where applicable. |

Hashing and manifest implementation are deferred.

## O. Validation Rules

Future validators must cover:

- schema check
- dtype and numeric check
- timezone UTC check
- monotonicity
- strict interval/session check
- duplicate timestamp check
- gap check
- OHLC bounds check
- volume/amount semantic check
- negative/zero policy check
- source/provenance completeness
- feature publication-time alignment for non-core features
- no future-data dependency

KRONOS-004 does not create validator code or executable schemas.

## P. Non-Core Feature Streams

These streams are not core OHLCVA:

- funding
- mark price
- index price
- premium
- open interest
- taker flow
- trade count
- corporate actions
- futures roll metadata
- FX spread/rollover
- calendar/session features

They require later feature contracts and publication-time rules before use. Publication-time alignment must prove that a feature value was available at or before the model input timestamp.

## Q. First Empirical Lean

The first empirical lean remains crypto BTC/ETH 1h. This is not a permanent crypto-only decision.

KRONOS-005 should specialize the first empirical market/timeframe contract. KRONOS-004 does not implement data fetching, adapters, validators, inference, or forecast artifacts for that target.

## R. Open Questions

- Confirm first empirical asset class and symbols.
- Confirm first timeframe.
- Resolve NQQ.
- Choose initial data source for BTC/ETH 1h.
- Decide whether to include mark/index/funding/open-interest later as separate features.
- Decide future equity/ETF, futures, and FX vendors.
- Decide whether `amount` is required or nullable for asset classes where turnover is not official.
- Decide exact enum values for asset identity fields.
- Decide gap manifest format in a later validator package.

## S. Forbidden Scope

Current forbidden scope remains:

- no data ingestion
- no API calls requiring credentials
- no live feeds
- no data adapter code
- no validator code
- no executable schema artifacts
- no forecast artifacts
- no strategy/risk/portfolio/execution work
- no dependency changes
- no model/tokenizer changes
- no tests/examples changes
- no README/LICENSE changes
- no data files
- no broker/exchange/private API integration
