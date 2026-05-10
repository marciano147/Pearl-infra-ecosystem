# Pearl App Thesis

Pearl app development should start from UTXO ecosystem needs, not EVM assumptions.

## App Tracks

1. Explorer/indexer API for blocks, transactions, addresses, mempool, miners, and data outputs.
2. Wallet SDK primitives for address validation, payment requests, and transaction lifecycle.
3. Pearl Pay payment links, merchant callbacks, and invoice tracking.
4. OTC market-data tools and liquidity/price visibility.
5. AI compute marketplace interfaces connecting Pearl-certified vLLM endpoints to users/operators.
6. Inscriptions/data anchoring using OP_RETURN/null-data if confirmed safe and indexable.
7. Escrow/multisig tooling only after script capabilities and security review are complete.

## Extension Wallet Position

A browser extension is not the first product unless a web app needs connect/sign/send flows. Build wallet SDK and app demand first, then extension/mobile wallet if justified.
