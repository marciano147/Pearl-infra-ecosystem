## Why

Pearl is an early Bitcoin-style UTXO L1 with a working node, wallet daemon, desktop wallet, SPV client, explorer endpoints, OTC activity, and Pearl-certified AI inference/mining models, but it lacks the reusable KaspaCom-owned app infrastructure needed to build wallets, explorers, payments, OTC tooling, and compute-marketplace products quickly.

KaspaCom should create a dedicated `pearl-infra` repository that vendors/forks only the relevant Pearl surfaces, documents the integration contracts, and provides a planned foundation for ecosystem apps before product code branches off into separate frontends/services.

## What Changes

- Create a new KaspaCom repository named `pearl-infra` as the canonical home for Pearl app infrastructure planning, adapters, SDKs, and service scaffolding.
- Define what Pearl upstream components are reused, forked, wrapped, or referenced, including `pearld`, `oyster`, wallet RPC docs, desktop wallet app patterns, address validation, Blockbook/explorer endpoints, SPV/light client, OTC APIs, and vLLM/Hugging Face mining/inference stack.
- Establish a modular architecture for the infrastructure needed by future Pearl apps:
  - chain node/RPC adapters and local/dev network helpers
  - indexer/explorer API service
  - wallet SDK and browser-app connectivity primitives
  - Pearl Pay/payment-link service
  - OTC/market-data ingestion service
  - AI compute marketplace control-plane interfaces
- Add goal-based execution rules, verification gates, and OpenSpec planning so implementation proceeds only from approved tasks.
- Avoid prematurely building a browser extension or marketplace before the infra contracts and user flows are validated.

## Capabilities

### New Capabilities
- `pearl-infra-repository`: Repository structure, governance, upstream Pearl source handling, local development standards, and quality gates.
- `pearl-chain-services`: Node/wallet/RPC integration layer covering `pearld`, `oyster`, JSON-RPC, wallet RPC, SPV/light client, and network configuration.
- `pearl-indexer-explorer`: Indexed chain data service and explorer API for blocks, transactions, addresses, mempool, miners, OP_RETURN/data outputs, and app consumption.
- `pearl-wallet-sdk`: Reusable wallet primitives for address validation, balance/history, transaction construction, signing/publishing flows, payment requests, and future browser/mobile wallets.
- `pearl-market-apps`: Application rails for Pearl Pay, OTC/market data, escrow flows, and AI compute marketplace integration without assuming EVM-style smart contracts.

### Modified Capabilities
- None.

## Impact

- New GitHub repository: `KASPACOM/pearl-infra`.
- New local working tree under `/home/coder/projects/pearl/pearl-infra`.
- New OpenSpec change under `openspec/changes/pearl-infra-ecosystem/`.
- Upstream reference: `pearl-research-labs/pearl` at inspected commit `0c8cef72da75d10ffd52ac20d3c0b075d9d9f1f7`.
- Initial repo should contain planning docs, integration map, copied minimal reference docs if license-compatible, and scaffolding only; no production wallet/explorer code until the OpenSpec tasks are approved.
