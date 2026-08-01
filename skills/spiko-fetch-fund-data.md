---
name: Fetch Spiko fund market data
description: Retrieve fund, share class, yield, NAV, and exchange-rate data from the open Spiko Public API.
api: openapi/spiko-public-openapi.json
operations: [Get all Funds, Get Share Class, Get Share Class Yield, Get Latest Net Asset Value, Get Latest Exchange Rate]
---

# Fetch Spiko fund market data

The Public API is open — **no authentication**. Base URL `https://public-api.spiko.io/v0`.

## Steps

1. List funds — `GET /funds/` (**Get all Funds**). Each fund exposes its share classes.
2. Inspect a share class — `GET /share-classes/{shareClassSymbol}` (**Get Share Class**).
3. Read the current yield — `GET /share-classes/{shareClassSymbol}/yield` (**Get Share Class Yield**).
4. Read the latest NAV — `GET /net-asset-values/{shareClassSymbol}/latest` (**Get Latest Net Asset Value**); use `GET /net-asset-values/{shareClassSymbol}` for history.
5. Convert currencies — `GET /exchange-rates/latest` (**Get Latest Exchange Rate**).

## Rules

- Read-only; safe for agents. No credentials, no side effects.
- Errors are Effect-tagged JSON with a `_tag` field (see `errors/spiko-problem-types.yml`); 404 means unknown symbol.
