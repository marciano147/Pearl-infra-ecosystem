# Pearl App Development Guide

This guide translates the Pearl chain model into practical KaspaCom app patterns.

## 1. Current Product Direction

Do not build a duplicate explorer or browser extension first.

Recommended sequence:

1. chain data adapters / typed RPC client;
2. wallet SDK primitives;
3. Pearl Pay/payment links;
4. OTC/market data tools;
5. miner/operator dashboards;
6. compute marketplace control plane;
7. browser/mobile connector only after real apps need signing.

## 2. App Architecture Pattern

```text
Frontend app
  |
  | calls KaspaCom API / SDK
  v
KaspaCom service layer
  |-- public Pearl explorer/Blockbook/OTC APIs for read-only data
  |-- pearld JSON-RPC for node-owned data when self-hosted
  |-- oyster wallet RPC only for user/owned wallet operations
  |-- app database for invoices, offers, users, callbacks, alerts
```

Early apps should use public read-only data and explicit user wallet actions. Avoid custodial flows unless explicitly approved.

## 3. Read-Only Chain Data Pattern

Use for dashboards, alerts, portfolio views, payment confirmation status, OTC widgets.

Flow:

1. normalize network config;
2. query public API/Blockbook/pearld;
3. validate and type responses;
4. cache short-lived public data;
5. expose clean app-facing API.

Example endpoints for our own future `services/chain-data-api`:

- `GET /health`
- `GET /chain/stats`
- `GET /blocks/recent`
- `GET /tx/:txid`
- `GET /address/:address`
- `GET /miners/top`

Best practices:

- Do not expose upstream response shapes directly to frontends.
- Add source metadata: `source`, `network`, `observedAt`, `height`.
- Cache but keep payment confirmation reads fresh.
- Treat public APIs as unreliable; add retries and fallback sources.

## 4. Pearl Pay Pattern

Use for payment links, checkout, invoice tracking, merchant callbacks.

Recommended flow:

1. merchant/app creates invoice off-chain;
2. backend generates or assigns receiving address;
3. user sends PRL from wallet;
4. backend watches address/tx;
5. invoice status updates from `pending` → `seen` → `confirmed`;
6. merchant callback fires idempotently.

Data model to plan:

```text
Invoice
- id
- network
- amountPrl
- receivingAddress
- expectedMemo/data? (only if verified)
- status: created | pending | seen | confirmed | expired | underpaid | overpaid
- txid
- confirmations
- merchantCallbackUrl
- createdAt / expiresAt / paidAt
```

Best practices:

- Use unique addresses when possible.
- Never rely only on frontend proof of payment.
- Require confirmation thresholds.
- Make callbacks idempotent with event IDs.
- Handle underpayment, overpayment, duplicate payment, late payment, and reorgs.
- Keep wallet seeds/private keys out of the payment service unless custody is explicitly approved.

## 5. Wallet SDK Pattern

The wallet SDK should hide chain quirks and expose stable app primitives.

Suggested package responsibilities:

`packages/pearl-rpc`:

- network constants;
- typed `pearld` JSON-RPC client;
- public API/Blockbook adapter;
- response validation;
- fixtures/tests.

`packages/pearl-sdk`:

- address validation;
- amount parsing/formatting;
- payment request schema;
- transaction lifecycle types;
- connector interface for future desktop/browser/mobile wallets.

Possible connector interface:

```ts
interface PearlWalletConnector {
  network(): Promise<'mainnet' | 'testnet' | 'testnet2' | 'simnet' | 'regtest'>;
  getAccounts(): Promise<{ address: string; label?: string }[]>;
  signMessage(message: string): Promise<string>;
  signTransaction(tx: unknown): Promise<unknown>;
  sendTransaction(tx: unknown): Promise<{ txid: string }>;
}
```

Do not promise this exact API until validated against Oyster/desktop-wallet flows.

## 6. OTC / Market Data Pattern

Use Pearl OTC public endpoints to build market visibility, alerts, and dashboards.

Public endpoints currently tracked in `docs/resources.md` include:

- `/stats`
- `/offers`
- `/trades/public?limit=5`
- `/trades/public/stats?window_days=30`
- `/chain/stats`
- `/chain/recent-blocks?limit=3`
- `/chain/top-miners?window_blocks=500&limit=10`

Best practices:

- Treat OTC data as supporting market data, not canonical chain truth.
- Separate public market reads from authenticated trading/admin endpoints.
- Do not scrape authenticated endpoints without approval.
- Add alert thresholds rather than noisy polling.

## 7. Compute Marketplace Pattern

Pearl’s whitepaper points toward useful compute and possible future settlement of compute workflows. Current evidence does not show general on-chain smart contracts.

Early compute marketplace should therefore be designed as a control plane:

- model catalog;
- GPU/operator registration;
- health checks;
- inference endpoint routing;
- metering;
- billing records;
- PRL payment/invoice settlement;
- miner/reward reporting dashboards.

Best practices:

- Keep compute job state off-chain initially.
- Use PRL payments for settlement where possible.
- Keep proof/mining data separate from user billing data.
- Do not claim trustless on-chain compute contracts until upstream supports it.

## 8. Data Anchoring Pattern

Potential use cases:

- receipt hashes;
- invoice commitment hashes;
- compute job result hashes;
- model/operator attestations.

But first verify:

- supported standard script/data output type;
- size limits;
- relay/mempool policy;
- explorer visibility;
- fee impact;
- wallet support for constructing such outputs.

Until verified, keep this as research, not product promise.

## 9. Security / Custody Rules

- Never commit seeds, private keys, wallet DBs, RPC passwords, `.env`, or generated certs.
- Do not build custodial payment flows by accident.
- Keep wallet signing client-side/user-controlled where possible.
- Use simnet/regtest/testnet before mainnet.
- Minimize RPC exposure; bind local nodes to localhost by default.
- Use TLS/auth for node and wallet RPC.
- Sanitize logs; never log seeds or private keys.
- Treat public APIs as untrusted input.
- Add explicit confirmation thresholds for payments.

## 10. Implementation Definition Of Done

A Pearl implementation task is not done until:

- source assumptions are documented with upstream file/URL;
- code has typed interfaces and validation;
- tests or fixtures cover happy path and failure path;
- no secrets/custody material are committed;
- docs explain how to run it locally;
- verification command output is recorded in PR/commit notes.

## 11. First App Recommendation

Build **Pearl Pay + chain-data SDK foundation** first.

Why:

- it creates real transaction demand;
- it does not require smart contracts;
- it benefits from existing explorer/public data;
- it gives a reason to later build a wallet connector/extension;
- it is easier to validate than a full compute marketplace.
