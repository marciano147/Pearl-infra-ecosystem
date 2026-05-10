# Next Steps

## Current Status

The repo is now a clean Pearl infrastructure knowledge base and planning repo.

Completed:

- GitHub repo created/pushed: https://github.com/marciano147/Pearl-infra-ecosystem
- OpenSpec proposal/design/specs/tasks added under `docs/openspec/pearl-infra-ecosystem/`
- Resource hub added: `docs/resources.md`
- Upstream manifest added: `docs/upstream-manifest.md`
- App thesis added: `docs/research/pearl-app-thesis.md`
- Upstream Pearl source linked as pinned submodule at `upstream/pearl`
- Goal-based contribution checklist added in `CONTRIBUTING.md`

## What We Still Need

### Must Decide

1. **First product track**
   - Recommended: **Explorer/indexer + wallet SDK foundation**.
   - Reason: every later app needs indexed chain data, transaction lookup, address history, payment confirmation, and normalized wallet primitives.

2. **Runtime stack**
   - Recommended: TypeScript monorepo with packages/services:
     - `packages/pearl-sdk`
     - `packages/pearl-rpc`
     - `services/indexer-api`
     - `services/market-data`
     - later `apps/explorer`

3. **Data source strategy**
   - Phase 1: use public Blockbook + `pearld` RPC contracts/mocks.
   - Phase 2: run our own Pearl node/indexer if needed.

4. **Product order**
   - Explorer/indexer API
   - Wallet SDK + payment request schema
   - Pearl Pay
   - OTC market tools
   - AI compute marketplace
   - Browser extension only after web apps require wallet signing

## Recommended Implementation Phase 1

Build the **read-only chain foundation** first.

Deliverables:

1. TypeScript workspace scaffold.
2. `packages/pearl-rpc`:
   - network config
   - typed `pearld` RPC client
   - typed Blockbook client
   - mocked fixtures/tests
3. `services/indexer-api`:
   - `/health`
   - `/chain/stats`
   - `/blocks/recent`
   - `/tx/:txid` contract stub
   - `/address/:address` contract stub
4. CI:
   - install
   - typecheck
   - test

Success criteria:

- `npm install` works.
- `npm run typecheck` passes.
- `npm test` passes.
- README explains how to run the skeleton.
- No secrets or mainnet wallet material.

## Recommended Implementation Phase 2

Build wallet/app primitives:

1. `packages/pearl-sdk` address validation wrapper.
2. Pearl payment request schema.
3. Transaction lifecycle interfaces.
4. Future connector interface without implementing browser extension yet.

## Recommended Implementation Phase 3

Pick first app:

- **Explorer UI** if we want broad ecosystem utility.
- **Pearl Pay** if we want transaction usage.
- **OTC market tool** if we want immediate PRL market relevance.
- **AI compute marketplace** if we can line up GPU operators and user demand.

## Recommendation

Implement Phase 1 now: **Explorer/indexer + RPC SDK scaffold**. It is the lowest-risk foundation and unlocks the most later products.
