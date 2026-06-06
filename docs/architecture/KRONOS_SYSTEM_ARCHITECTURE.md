# Kronos System Architecture

## Accepted Stance

Accepted architecture stance:

Kronos forecast-artifact core + custom multi-asset data/research/risk layers + hybrid tooling where useful + execution routers much later.

The system is Kronos-centered, multi-asset over time, forecast-artifact-centered, custom-runner / hybrid, and execution much later. It is not BTC-only, not crypto-only, and not generic-framework-first.

## High-Level Flow

Data -> inference -> forecast artifact -> signal/evaluation -> risk/portfolio -> execution much later.

Each boundary must preserve authority separation. Forecasts may inform research and evaluation, but they do not place orders, size positions, change risk, allocate portfolios, or promote strategies.

## Multi-Asset Direction

The architecture must preserve long-term support for:

- crypto
- equities / ETFs
- Nasdaq-100 exposure: QQQ / NQ / MNQ still unresolved
- index futures
- FX
- possibly other liquid markets later

The first empirical lean is crypto BTC/ETH 1h. This is a starting point for evidence, not a permanent crypto-only architecture decision.

## Forecast Artifact Boundary

Forecast artifacts are the contract between Kronos inference and downstream research. They should carry input identity, model identity, runtime settings, timestamps, asset/timeframe identity, forecast horizon, generated values, validation status, and lineage.

This boundary exists so downstream signal, evaluation, risk, and execution layers cannot silently treat model output as direct authority.

## Architecture Choices

Option B + D is accepted: a Kronos forecast-artifact core with custom multi-asset data/research/risk layers and hybrid tooling where useful.

Avoid a Kronos-centered monolith because the project needs explicit boundaries, validation, artifact lineage, and future safety gates. Avoid BTC-only or crypto-only architecture because multi-asset support is a core direction. Avoid a generic framework-first replacement because Kronos is the center of this repo and generic frameworks should support, not replace, the core.

Execution routers are later because runtime proof, data contracts, forecast artifacts, evaluation, baselines, risk simulation, and paper/live design must come first.

## Explicit Avoids

- Kronos-centered monolith
- BTC-only architecture
- crypto-only architecture
- generic framework-first replacement
- direct forecast-to-order path
- direct model-to-risk path
- execution/risk/OMS work before explicit approval
