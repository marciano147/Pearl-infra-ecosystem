# Pearl Development Docs

Start here if you are new to Pearl or this repo.

## Guides

1. [`local-dev-guide.md`](local-dev-guide.md) — clone, initialize submodule, install toolchains, build upstream Pearl, run local node/wallet, use public APIs.
2. [`pearl-chain-primer.md`](pearl-chain-primer.md) — how Pearl works: UTXO ledger, node/wallet split, RPCs, mining, SPV, and current smart-contract limitations.
3. [`pearl-app-development.md`](pearl-app-development.md) — how KaspaCom should build Pearl apps: chain-data adapters, wallet SDK, Pearl Pay, OTC tools, compute marketplace control plane.

## Golden Rules

- Treat Pearl as Bitcoin-style UTXO until proven otherwise.
- Do not assume EVM/Solidity/WASM smart contracts.
- Do not duplicate Pearl’s public explorer UI unless a product gap is proven.
- Build SDK/data/payment rails first; browser extension later when apps need signing.
- Never commit seeds, private keys, wallet DBs, RPC passwords, generated certs, or `.env` files.
