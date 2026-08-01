---
name: Onboard an investor and place a deposit (Distributor API)
description: Create an investor, open an account, and submit a deposit order on their behalf as a Spiko distributor.
api: openapi/spiko-distributor-openapi.json
operations: [investors.createInvestorV1, accounts.createAccount, accounts.getDepositInstructions, depositOrders.createDepositOrder, depositOrders.getDepositOrder]
---

# Onboard an investor and place a deposit (Distributor API)

Base URL `https://distributor-api.spiko.io` (sandbox `https://distributor-api.preprod.spiko.io`). Auth: **HTTP Basic** with `client_id`:`client_secret`.

## Steps

1. Create the investor — `POST /v1/investors` (**investors.createInvestorV1**). Use the v1 endpoint; `POST /v0/investors` is deprecated. KYC/compliance runs for real, even in sandbox.
2. Open an account for a share class — `POST /v0/accounts` (**accounts.createAccount**).
3. Get funding instructions — `GET /v0/accounts/{accountId}/deposit-instructions` (**accounts.getDepositInstructions**).
4. Submit the deposit order — `POST /v0/deposit-orders/` (**depositOrders.createDepositOrder**). Send an `Idempotency-Key` header (8-255 alphanumeric chars) so retries never double-book.
5. Track it — `GET /v0/deposit-orders/{id}` (**depositOrders.getDepositOrder**), or subscribe to the `deposit-order.funded` / `deposit-order.executed` webhooks.

## Rules

- **Idempotency**: every create POST accepts `Idempotency-Key`; reuse the same key on retry with an identical body (`conventions/spiko-conventions.yml`).
- `InvestorNotReadyError` (422) means onboarding/KYC is incomplete — do not order until the investor is compliant.
- Money movement is simulated in sandbox; contact Spiko to trigger executions.
- Errors: Effect-tagged JSON `_tag` (`errors/spiko-problem-types.yml`).
