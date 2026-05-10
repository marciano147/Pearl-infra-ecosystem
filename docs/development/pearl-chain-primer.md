# Pearl Chain Primer

This primer explains Pearl as currently observed from the upstream repo and public docs. It is written for app developers, not protocol researchers.

## 1. What Pearl Is

Pearl is an L1 blockchain based on Proof-of-Useful-Work (PoUW). Instead of mining by pure hash grinding, Pearl aims to mine as a by-product of useful matrix multiplication work used by AI workloads.

From an app developer perspective, Pearl currently behaves like a Bitcoin-style UTXO chain:

- full node: `pearld`;
- wallet daemon: `oyster`;
- CLI/RPC tool: `prlctl`;
- light client: `spv`;
- mining gateway: `pearl-gateway`;
- vLLM miner: `vllm-miner`.

Current evidence does **not** show an EVM/Solidity/WASM smart-contract VM. Do not design apps as if Pearl has Ethereum-style contracts unless future upstream evidence proves it.

## 2. Mental Model

```text
Users / Apps
    |
    | wallet SDK, payment request, address validation
    v
Oyster wallet daemon ---- websocket/RPC ---- pearld full node ---- P2P network
    |                                               |
    | signs/funds/publishes txs                     | blocks, mempool, chain RPC
    v                                               v
Wallet DB / keys                              UTXO set / chain index

AI/vLLM miner ---- pearl-gateway ---- getblocktemplate / submitblock ---- pearld
```

Separate responsibilities:

- `pearld` knows the chain, mempool, peers, block validation, mining templates, and transaction relay.
- `oyster` knows wallet accounts, keys, balances, funding, signing, publishing, and wallet transaction history.
- `spv` is for lightweight clients that do not want to run a full node.
- `pearl-gateway` connects mining/inference systems to the node.

## 3. UTXO Basics

Pearl uses a UTXO ledger model.

A transaction:

1. consumes one or more previous unspent outputs as inputs;
2. creates one or more new outputs;
3. signs the inputs to prove the spender controls the coins;
4. is relayed to the mempool;
5. becomes final once mined into enough blocks.

Important difference from account chains:

- There is no account balance stored as one mutable number on-chain.
- A wallet balance is the sum of spendable UTXOs controlled by that wallet.
- Sending PRL usually creates change back to the sender.
- Apps that need invoice/payment state usually need an indexer or wallet transaction monitor.

## 4. Addresses And Scripts

Observed Pearl wallet/node code supports Bitcoin-like script concepts:

- Taproot/P2TR addresses;
- P2SH/multisig references;
- txscript-style scripts;
- raw transaction creation/signing;
- address validation;
- null-data / OP_RETURN style data should be researched and verified before product use.

Practical app rules:

- Generate a new receiving address per invoice/session when possible.
- Do not reuse addresses in hosted payment apps unless there is a clear reason.
- Validate addresses before accepting or displaying payment instructions.
- Treat complex script usage as advanced/protocol work requiring review.

## 5. Node RPC Surface

`pearld` exposes JSON-RPC over HTTP POST and websocket. Websocket is preferred for long-lived clients and notifications.

Key characteristics:

- TLS enabled by default;
- RPC disabled unless credentials are configured;
- standard Bitcoin-like RPC plus Pearl-specific/mining behavior;
- wallet functionality is not inside `pearld`; use `oyster` for wallet operations.

Useful read-only RPCs:

- `getblockcount`
- `getbestblockhash`
- `getblockhash`
- `getblock`
- `getblockheader`
- `getchaintips`
- `getdifficulty`
- `getmempoolinfo`
- `getmininginfo`
- `getnetworkhashps`
- `getrawmempool`
- `getrawtransaction`
- `validateaddress`

Useful transaction RPCs:

- `createrawtransaction`
- `decoderawtransaction`
- `decodescript`
- `sendrawtransaction`

Mining RPCs:

- `getblocktemplate`
- `submitblock`
- `setgenerate` / `getgenerate` where supported/configured.

## 6. Wallet RPC Surface

`oyster` is the wallet daemon. It derives keys, tracks wallet state, funds transactions, signs transactions, and publishes transactions through the node.

Important gRPC methods from upstream docs:

- `WalletExists`
- `CreateWallet`
- `OpenWallet`
- `CloseWallet`
- `StartConsensusRpc`
- `Balance`
- `GetTransactions`
- `NextAddress`
- `FundTransaction`
- `SignTransaction`
- `PublishTransaction`
- `TransactionNotifications`

Typical app transaction lifecycle:

1. validate network and destination address;
2. create/prepare a transaction output;
3. ask wallet to fund it with UTXOs;
4. ask wallet to sign it;
5. publish the signed transaction;
6. watch transaction notifications / chain data for confirmations.

## 7. SPV / Light Client

Pearl includes a light client using compact block filters.

Why it matters:

- future browser/mobile wallets may not want a full node;
- apps can scan relevant addresses/outputs more privately than asking a centralized API for everything;
- `GetUtxo` and `Rescan` are important primitives for wallet recovery and confirmation logic.

For early KaspaCom products, prefer public read-only APIs or a backend adapter first. Use SPV later if we need better privacy or client-side verification.

## 8. Proof-of-Useful-Work / Mining

Pearl's mining stack is centered on useful matrix multiplication work.

Observed components:

- `zk-pow` — proof circuit/verifier;
- `py-pearl-mining` — Python bindings;
- `miner/pearl-gateway` — bridge from miner to node;
- `miner/vllm-miner` — vLLM plugin/miner;
- Hugging Face models under `pearl-ai/*`.

High-level flow:

1. `pearld` provides block templates.
2. `pearl-gateway` keeps a work cache and miner-facing RPC.
3. vLLM/inference process performs matrix multiplication work.
4. miner extracts/proves eligible work.
5. gateway submits candidate blocks/proofs to `pearld`.

App implication:

- compute marketplace products are mostly off-chain workflow + metering + settlement UX at first;
- do not assume compute jobs are currently represented as general on-chain smart contracts;
- miner/operator dashboards can be useful even without on-chain compute contracts.

## 9. Existing Public Surfaces

Pearl already has:

- website/whitepaper: `https://pearlresearch.ai/`;
- explorer: `https://explorer.pearlresearch.ai/?network=mainnet`;
- OTC app: `https://pearl-otc.com/`;
- OTC/chain API: `https://api.pearl-otc.com`;
- Blockbook-style endpoints referenced by wallet source;
- Hugging Face org: `https://huggingface.co/pearl-ai`.

This means KaspaCom should not blindly clone existing UI. Build missing app infrastructure and use-case-specific products.

## 10. What Pearl Apps Need

Because Pearl is UTXO-first, useful apps are likely:

- wallet SDK and connector;
- payment links / checkout / invoices;
- merchant callbacks and confirmation tracking;
- portfolio/activity views;
- OTC/market data dashboards;
- miner/operator dashboards;
- compute marketplace control plane;
- address/transaction alerts;
- data anchoring if OP_RETURN/null-data support is verified.

Less likely until proven:

- EVM-style DeFi;
- ERC-20-like token launchers;
- Solidity smart contracts;
- on-chain AMMs/lending;
- NFT platforms in the EVM sense.

## 11. Developer Best Practices

- Start read-only: chain stats, recent blocks, tx lookup, address lookup.
- Add wallet signing only after the app flow is clear.
- Keep custody out of early products.
- Use testnet/simnet/regtest for all experiments.
- Store app state off-chain when Pearl does not natively provide smart contracts.
- Build idempotent payment confirmation logic; chain APIs can lag or reorg.
- Require confirmation thresholds for payments.
- Never trust a single frontend state; verify via node/indexer/wallet.
- Separate public APIs from authenticated/custodial/admin APIs.
- Document every upstream assumption with source file and commit.
