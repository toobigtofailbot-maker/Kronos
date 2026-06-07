# Crypto 1h First Empirical Target

## A. Scope

This is a docs-only contract for the first empirical market/timeframe target. It defines the BTC/ETH 1h Binance USDT-M perpetual data contract before any future data adapter, validator, storage, inference, or forecast-artifact package.

This document does not fetch data, create data files, create adapters, create validators, create executable schemas, run inference, create forecast artifacts, or authorize strategy/risk/portfolio/execution work.

## B. Source-Truth Hierarchy

1. Kronos repo input/output behavior
2. `docs/data/MULTI_ASSET_DATA_CONTRACT.md`
3. `docs/data/KRONOS_INPUT_CONTRACT.md`
4. Official Binance USD-M Futures documentation
5. Future data samples
6. Chat memory

Repo truth wins over this design if later current code or accepted docs conflict with it.

## C. Target Universe

| Field | First empirical target |
| --- | --- |
| Symbols | `BTCUSDT`, `ETHUSDT` |
| Asset class | `crypto_perp` |
| Venue | `binance_usdt_m` |
| Market type | perpetual |
| Quote/margin family | USDT-M |
| Timeframe | `1h` |

This is the accepted near-term proof path. The long-term goal remains multi-asset opportunity discovery. This is the first empirical target only; it is not a permanent crypto-only decision and not a BTC-only decision.

## D. Venue and Instrument Type

Intended identity fields for future normalized rows:

| Field | BTC target | ETH target |
| --- | --- | --- |
| `asset_id` | `binance_usdt_m_perp_BTCUSDT` | `binance_usdt_m_perp_ETHUSDT` |
| `symbol_native` | `BTCUSDT` | `ETHUSDT` |
| `symbol_canonical` | `BTCUSDT_PERP_BINANCE_USDTM` | `ETHUSDT_PERP_BINANCE_USDTM` |
| `instrument_type` | perpetual | perpetual |
| `market_type` | perpetual | perpetual |
| `venue` | `binance_usdt_m` | `binance_usdt_m` |
| `source` | `binance_usdm_futures_rest_docs` | `binance_usdm_futures_rest_docs` |

The canonical symbol convention is provisional until a later implementation package locks exact enum and naming mechanics.

## E. Timeframe

| Field | Requirement |
| --- | --- |
| `timeframe` | `1h` |
| Bar schedule | 24/7 UTC continuous |
| Expected interval | One hour between consecutive open times |

No market-closed periods are expected for the 24/7 crypto calendar. Source outages, missing candles, maintenance incidents, and source-side interruptions must be detected rather than silently repaired.

## F. Core Price Stream

The core price stream is Binance USD-M Futures trade-price kline/candlestick data for `/fapi/v1/klines`.

Do not use mark price klines, index price klines, or premium index klines as the core OHLCVA stream for this first target. Do not combine multiple price streams into one OHLCVA record.

## G. Official Binance Kline Field Mapping

Binance USD-M Futures kline rows are uniquely identified by open time and include the response fields below. The future adapter must map only the selected trade-price kline fields into core OHLCVA.

| Binance kline position / field | Canonical target | Core OHLCVA? | Policy |
| --- | --- | --- | --- |
| Open time | `raw_open_time_ms`, `bar_open_time_utc` | Identity/time | Convert from milliseconds to UTC; open time defines kline identity. |
| Open | `open` | Yes | Use as the opening price from the trade-price stream. |
| High | `high` | Yes | Use as the high price from the trade-price stream. |
| Low | `low` | Yes | Use as the low price from the trade-price stream. |
| Close | `close` | Yes | Use as the close price from the trade-price stream. |
| Volume | `volume` | Yes | Base asset volume. |
| Close time | `raw_close_time_ms`, `bar_close_time_utc` | Time | Convert from milliseconds to UTC. |
| Quote asset volume | `amount` | Yes | Quote asset volume. |
| Number of trades | Excluded | No | Non-core; do not overload into volume or amount. |
| Taker buy base asset volume | Excluded | No | Non-core taker-flow field. |
| Taker buy quote asset volume | Excluded | No | Non-core taker-flow field. |
| Ignore | Ignored / rejected as core | No | Must not be used as a core field. |

For this first target, `volume` means base asset volume and `amount` means quote asset volume. Do not use number of trades, taker buy volumes, or the ignore field as `volume` or `amount`.

## H. Canonical Output View

Future normalized data for this target should conceptually expose:

| Column | Requirement |
| --- | --- |
| `asset_id` | Stable project identifier. |
| `asset_class` | `crypto_perp`. |
| `venue` | `binance_usdt_m`. |
| `source` | Source family for Binance USD-M Futures trade-price klines. |
| `symbol_native` | `BTCUSDT` or `ETHUSDT`. |
| `symbol_canonical` | Provisional canonical symbol. |
| `instrument_type` | `perpetual`. |
| `market_type` | `perpetual`. |
| `timeframe` | `1h`. |
| `bar_open_time_utc` | UTC opening timestamp. |
| `bar_close_time_utc` | UTC closing timestamp. |
| `open`, `high`, `low`, `close` | Trade-price OHLC. |
| `volume` | Base asset volume. |
| `amount` | Quote asset volume. |
| `source_name` | Official/source family name. |
| `source_documentation_url` | Official documentation URL used for semantics. |
| `raw_source_path_or_reference` | Future raw source locator. |
| `raw_data_hash` | Future raw source hash. |
| `normalized_data_hash` | Future normalized output hash. |
| `normalization_policy_version` | Future normalization policy version. |
| `contract_version` | This contract version or successor. |
| `quality_flags` | Explicit quality flags. |
| `known_gaps_reference` | Future gap-manifest reference. |

Hashes, manifests, raw references, and storage mechanics are future implementation details.

## I. Timestamp Policy

- Open time and close time from the Binance response must be converted to UTC.
- Canonical storage must use UTC.
- Open time defines kline identity.
- Bars must be strictly increasing by open time within each symbol.
- `1h` bars must have exactly one-hour open-time spacing unless a documented gap is recorded.
- `bar_close_time_utc` must come from the source close time field after UTC conversion.
- Do not derive `y_timestamp` from observed future bars.
- Future `y_timestamp` must be generated mechanically from the validated schedule.

## J. Volume and Amount Policy

- `volume` is required and maps to Binance base asset volume.
- `amount` is required and maps to Binance quote asset volume.
- Missing `volume` is rejected.
- Missing `amount` is rejected.
- Negative `volume` or `amount` is rejected.
- Do not rely on the current Kronos repo fallback that zero-fills missing volume or synthesizes amount from volume and mean OHLC price.

The repo predictor can tolerate missing fields by filling or deriving them, but this project contract is stricter for the first empirical target.

## K. Non-Core Fields and Deferred Feature Streams

Non-core fields and streams include:

- number of trades
- taker buy base asset volume
- taker buy quote asset volume
- mark price
- index price
- premium index
- funding rate
- open interest
- basis
- long/short ratios
- liquidations, if later considered

These may become separate feature streams later only with explicit feature contracts and publication-time alignment. They must not be added to core OHLCVA in KRONOS-005.

## L. Missing, Duplicate, Gap, and Repair Policy

Future validation must reject:

- duplicate open times
- non-monotonic open times
- NaN or infinity values
- OHLC inconsistency
- negative `volume` or `amount`
- missing required OHLCVA fields
- unknown or mismatched symbol, interval, venue, or source family

Future validation must detect missing `1h` intervals. The default repair policy is reject. Silent interpolation is forbidden. Forward-filling OHLC into missing tradable bars is forbidden.

Any future repair must be explicit, logged, justified, reproducible, and not future-looking. A future gap manifest is required for accepted gaps, but KRONOS-005 does not implement it.

## M. Required Provenance

Future normalized rows or manifests must record:

- official source URL used for field semantics
- endpoint family
- symbol
- interval
- download/request time when implemented
- raw source reference when implemented
- raw data hash when implemented
- normalized data hash when implemented
- normalization policy version
- contract version
- quality flags
- known gaps reference

## N. Future Validator Checklist

Future validator checks for this target:

- symbol allowed: `BTCUSDT` or `ETHUSDT`
- interval allowed: `1h`
- venue/source allowed: Binance USDT-M trade-price klines
- required raw fields present
- required canonical fields present
- UTC timestamp normalization
- strict one-hour open-time interval
- open time and close time present
- duplicate open time rejection
- gap detection
- OHLC consistency
- nonnegative `volume` and `amount`
- `amount` present
- no non-core fields in core OHLCVA
- provenance completeness
- no future-derived `y_timestamp`

KRONOS-005 does not implement validator code.

## O. Future Adapter Requirements

Future data adapters for this target must:

- accept only source data matching this contract
- map Binance raw fields to canonical fields
- preserve raw source reference
- produce the normalized OHLCVA view
- emit quality flags and gap references
- fail closed on invalid input
- never silently repair
- never fetch live/private data
- never use credentials
- never call execution/trading APIs

KRONOS-005 does not implement adapters.

## P. Documentation-Only Example

This table is documentation-only and uses fake values. It is not market data and must not be copied into a data file.

| asset_id | asset_class | venue | symbol_native | timeframe | bar_open_time_utc | bar_close_time_utc | open | high | low | close | volume | amount | quality_flags |
| --- | --- | --- | --- | --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| `binance_usdt_m_perp_BTCUSDT` | `crypto_perp` | `binance_usdt_m` | `BTCUSDT` | `1h` | `2026-01-01T00:00:00Z` | `2026-01-01T00:59:59.999Z` | `100000.00` | `100500.00` | `99500.00` | `100100.00` | `123.456` | `12345678.90` | `[]` |

## Q. Official Sources Inspected

| Source name | Exact URL | Fields/semantics used | Sufficient? | Unresolved ambiguity |
| --- | --- | --- | --- | --- |
| Binance USD-M Futures Kline/Candlestick Data | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Kline-Candlestick-Data> | Trade-price kline endpoint family; open-time identity; request `symbol` and `interval`; response fields open time, OHLC, volume, close time, quote asset volume, number of trades, taker buy base/quote volume, ignore. | Yes for raw kline field mapping. | Does not by itself lock project symbol set, storage format, or acquisition method. |
| Binance USD-M Futures Exchange Information | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Exchange-Information> | Symbol-information endpoint family; example `contractType`, `status`, base/quote/margin assets, and timezone. | Sufficient as contextual docs for future symbol/status checks. | This package did not call the endpoint and therefore did not verify current live BTCUSDT/ETHUSDT status. |
| Binance USD-M Futures Common Definition | <https://developers.binance.com/docs/derivatives/usds-margined-futures/common-definition> | Base asset and quote asset terminology; `PERPETUAL` contract type; `TRADING` status enum; kline interval enum including `1h`. | Yes for interval enum and terminology. | Does not provide source data. |
| Binance USD-M Futures Mark Price Kline/Candlestick Data | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Mark-Price-Kline-Candlestick-Data> | Mark price klines are separate bars identified by open time; non-price payload positions are marked ignore in the response example. | Yes for non-core classification. | Future feature alignment remains deferred. |
| Binance USD-M Futures Index Price Kline/Candlestick Data | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Index-Price-Kline-Candlestick-Data> | Index price klines are separate bars for a pair, identified by open time; non-price payload positions are ignored in the response example. | Yes for non-core classification. | Future feature alignment remains deferred. |
| Binance USD-M Futures Premium Index Kline Data | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Premium-Index-Kline-Data> | Premium index klines are separate bars identified by open time. | Yes for non-core classification. | Future feature alignment remains deferred. |
| Binance USD-M Futures Get Funding Rate History | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Get-Funding-Rate-History> | Funding-rate records include symbol, funding rate, funding time, and associated mark price. | Yes for non-core classification. | Publication-time and feature contract remain deferred. |
| Binance USD-M Futures Open Interest | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Open-Interest> | Open interest endpoint returns present open interest for a symbol with transaction time. | Yes for non-core classification. | Historical alignment and feature contract remain deferred. |
| Binance USD-M Futures Taker Buy/Sell Volume | <https://developers.binance.com/docs/derivatives/usds-margined-futures/market-data/rest-api/Taker-BuySell-Volume> | Taker buy/sell volume is a separate endpoint with `period` enum including `1h`, buy/sell ratio, buy volume, sell volume, and timestamp. | Yes for non-core classification. | This is not the same as kline taker buy base/quote fields; future feature design remains deferred. |

## R. Open Questions

- Confirm whether `BTCUSDT` and `ETHUSDT` remain the first empirical symbols after KRONOS-005.
- Confirm whether `1h` remains the first timeframe after KRONOS-005.
- Decide raw data source acquisition method in a later package.
- Decide raw file format in a later package.
- Decide normalized storage format in a later package.
- Decide gap manifest format in a later package.
- Decide whether mark, index, funding, open interest, taker-flow, basis, long/short ratios, or liquidation streams become separate feature streams later.
- Decide whether Binance data source should be REST API, bulk data archive, vendor mirror, or manually supplied sample in future packages.
- Decide exact canonical symbol and enum conventions.

## S. Forbidden Scope

KRONOS-005 forbids:

- data fetching
- market-data endpoint calls
- data files
- data adapters
- validator code
- executable schemas
- runtime audit
- model inference
- forecast artifacts
- strategy/risk/portfolio/execution work
- credentials
- broker/exchange/private API use
