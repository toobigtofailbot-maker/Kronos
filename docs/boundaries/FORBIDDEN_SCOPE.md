# Forbidden Scope

These boundaries are active until a later approved work package explicitly changes them.

## Currently Forbidden

- broker/exchange API integration
- API keys / credential handling
- order execution
- paper/live trading
- OMS
- reconciliation
- automated risk controls
- portfolio allocator
- fine-tuning
- full multi-market ingestion engine
- model/tokenizer refactor
- strategy optimization
- forecast-to-order logic
- direct model-to-risk changes
- hardcoded risk policy
- hardcoded Kelly sizing
- hardcoded daily drawdown threshold
- Kronos inference service
- live WebSocket feeds
- private broker/exchange API calls

## Permanent Model-Authority Boundary

Kronos forecasts may inform research.

Kronos forecasts may not directly place orders.

Kronos forecasts may not directly size positions.

Kronos forecasts may not directly change risk.

Kronos forecasts may not directly promote strategies.

## KRONOS-001 Drift Is Forbidden

KRONOS-001 must not drift into:

- runtime audit
- environment setup
- dependency changes
- model inference
- Hugging Face downloads
- test changes
- data adapter creation
- forecast artifact creation
- strategy/signal logic
- risk logic
- portfolio logic
- execution logic

If safe implementation requires touching these areas, stop and report instead of broadening scope.
