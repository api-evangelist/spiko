---
name: Deposit and check portfolio (Investor API)
description: As a direct Spiko investor, list accounts, place a deposit order, and read your portfolio and yields.
api: openapi/spiko-investor-openapi.json
operations: [investors.getInvestor, accounts.getAccounts, accounts.getDepositInstructions, depositOrders.createDepositOrder, portfolio.getPortfolio, yields.getYieldHistory]
---

# Deposit and check portfolio (Investor API)

Base URL `https://investor-api.spiko.io/v1` (sandbox `https://investor-api.preprod.spiko.io/v1`). Auth: **HTTP Basic** (`client_id`/`client_secret`) or **OAuth 2.0** bearer token (authorization code + PKCE).

## Steps

1. Confirm identity — `GET /investors/me` (**investors.getInvestor**).
2. List accounts — `GET /accounts/` (**accounts.getAccounts**); pick the target `accountId`.
3. Get funding instructions — `GET /accounts/{accountId}/deposit-instructions` (**accounts.getDepositInstructions**).
4. Place a deposit — `POST /deposit-orders/` (**depositOrders.createDepositOrder**) with an `Idempotency-Key` header. (Use deposit/withdrawal, not the deprecated subscription/redemption operations.)
5. Read holdings — `GET /portfolio/` (**portfolio.getPortfolio**) and totals via `GET /portfolio/totals`.
6. Review accrued yield — `GET /yields/history` (**yields.getYieldHistory**).

## Rules

- Prefer OAuth for scoped, revocable access; request the `offline` scope for a refresh token.
- Idempotency-Key on all create POSTs (`conventions/spiko-conventions.yml`).
- `InsufficientBalanceError` / `DepositNotAllowedError` return 409; handle before retrying.
- List endpoints are cursor-paginated (`cursor`, `limit`, `direction`).
