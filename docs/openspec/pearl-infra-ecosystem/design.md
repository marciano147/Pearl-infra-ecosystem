## Context

Pearl currently exposes enough low-level infrastructure to build an ecosystem, but not enough app-facing infrastructure for KaspaCom products. The inspected upstream repository includes `pearld` full node, `oyster` wallet daemon, SPV/light client, desktop wallet, address validation package, JSON-RPC/wallet RPC docs, OP_RETURN/null-data support, Taproot/P2TR support, Blockbook-style endpoints, and vLLM mining/inference components. Pearl OTC also exposes public market and chain APIs.

Pearl is not an EVM chain in the inspected codebase. Treat it as a Bitcoin-like UTXO L1: apps must be built around wallet/RPC/indexer/payment/escrow/data primitives rather than account smart contracts.

Stakeholders: KaspaCom/Sione as product owner, future Pearl users, GPU operators, PRL holders/traders, app developers, and KaspaCom operators maintaining infra.

## Goals / Non-Goals

**Goals:**
- Create `KASPACOM/pearl-infra` as the canonical infrastructure repo for Pearl app development.
- Capture the upstream Pearl surfaces that matter for wallets, payments, OTC tooling, data anchoring, chain-data views, and AI compute marketplace work.
- Define adapters and interfaces before product implementation so wallet/payment/market/compute apps share the same primitives.
- Provide OpenSpec-governed tasks and goal-based execution gates before coding.
- Keep upstream Pearl source handling clear: reference, submodule, vendored docs, or forked code must be explicit and license-aware.

**Non-Goals:**
- Do not build a browser extension before there is a web-app use case requiring wallet signatures.
- Do not assume EVM-style token/DeFi/smart-contract capabilities unless later research proves equivalent Pearl scripting/indexer primitives.
- Do not fork the whole Pearl monorepo blindly into KaspaCom source.
- Do not run mainnet infrastructure or custody user funds during the planning/bootstrap phase.
- Do not expose secrets, RPC passwords, wallet seeds, or private keys in repo/docs/tests.

## Decisions

1. **Repo role: infrastructure foundation, not first product app**
   - Decision: `pearl-infra` owns adapters, SDKs, service skeletons, docs, specs, and integration tests. Product UIs can live in the same monorepo initially only if they are thin prototypes.
   - Rationale: Pearl needs common building blocks before separate wallet/pay/market/compute apps diverge.
   - Alternative considered: build extension wallet first. Rejected for now because no active dapp surface exists that needs browser signatures.

2. **Upstream Pearl handling: reference/submodule first, selective extraction second**
   - Decision: keep upstream commit and file map documented; use a git submodule or scriptable fetch for `pearl-research-labs/pearl`; copy only minimal docs/types/examples that are license-compatible and needed for stable local development.
   - Rationale: avoids a stale, unmaintainable fork while preserving reproducibility.
   - Alternative considered: full fork. Deferred unless KaspaCom needs to modify `pearld`, `oyster`, or wallet internals.

3. **Architecture: layered app infrastructure**
   - Decision: split into layers:
     - `chain`: `pearld` RPC, network config, health, block/tx/mempool calls
     - `wallet`: `oyster` RPC, address validation, transaction construction/sign/publish flows
     - `indexer`: chain ingestion, address/tx/block/miner views, OP_RETURN decoding
     - `apps`: Pearl Pay, OTC/market data, compute marketplace interfaces
     - `ops`: docker/devnet, observability, deployment docs
   - Rationale: separates UTXO chain mechanics from product flows.

4. **Indexer-first app enablement**
   - Decision: app-facing chain-data adapters should be prioritized alongside chain/wallet adapters because most later apps need address history, confirmations, mempool state, miner data, and OP_RETURN/data lookup. Since Pearl already has a public explorer, do not build a duplicate explorer UI unless a specific product gap is proven.
   - Rationale: Pearl RPC is node-oriented; apps need query-oriented APIs.

5. **Wallet SDK before extension wallet**
   - Decision: build reusable wallet SDK primitives before any browser extension/mobile wallet.
   - Rationale: extension value depends on apps needing connect/sign. SDK primitives are useful immediately for Pearl Pay, market tools, chain-data widgets, and future wallets.

6. **Skill-based execution gates**
   - Decision: every implementation task must list the applicable skill(s), success criteria, failure escalation path, and verification command before being marked complete.
   - Rationale: keeps the repo disciplined and prevents silent partial implementation.

## Risks / Trade-offs

- [Pearl upstream changes quickly] → Pin upstream commit, document update process, and add compatibility tests for RPC/address/transaction behavior.
- [No smart-contract VM limits app ideas] → Frame products around UTXO payments, escrow, indexing, OP_RETURN/data anchoring, and AI compute; do not promise EVM-like dapps.
- [Wallet/custody risk] → Avoid mainnet custody in early phases; use testnet/regtest/dev fixtures; never commit seeds/private keys.
- [Chain-data scope creep] → Reuse Pearl explorer/Blockbook first; start with read-only blocks/tx/address/mempool/miner APIs before rich analytics or any custom explorer UI.
- [Compute marketplace is demand-side hard] → Keep compute marketplace as an interface/rail until user demand and operator supply are validated.
- [Fork maintenance burden] → Prefer adapters and submodules over copying upstream source.

## Migration Plan

1. Bootstrap local `pearl-infra` repo with README, integration map, upstream manifest, OpenSpec docs, and no production code.
2. Create GitHub repository under KASPACOM after confirming private/public posture; default to private if not specified.
3. Add upstream Pearl reference via submodule or fetch script pinned to `0c8cef72da75d10ffd52ac20d3c0b075d9d9f1f7`.
4. Implement adapters and tests in small phases after OpenSpec approval.
5. Split product apps into separate repos only after infra interfaces stabilize.

Rollback: remove the GitHub repo/local working tree if no production code has shipped; otherwise archive the OpenSpec change and freeze the repo as research-only.

## Open Questions

- Should `KASPACOM/pearl-infra` be private initially or public from day one?
- Should upstream Pearl be added as a git submodule, subtree, or fetched by script?
- Which first app should validate the infra: Pearl Pay, OTC market app, compute marketplace, or chain-data widgets?
- Does KaspaCom want to run public Pearl nodes/indexers, or only provide app code initially?
