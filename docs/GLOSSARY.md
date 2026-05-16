# Pearl Glossary

Pearl-specific terms, in alphabetical order. Anything Bitcoin-standard is only included if Pearl uses it differently.

## A

**Address (Pearl)** — Bech32m-encoded, **Taproot-only** (P2TR). Prefix `prl1p…` on mainnet. No legacy P2PKH / P2SH-wrapped addresses. Validate before display.

## B

**BLAKE3** — Pearl's hash for mining tile-opening: `BLAKE3(seed, M_{i,j}) < 2^(256-b)` decides which output tiles must be opened (proof revealed) in a candidate block.

**Block subsidy** — Reward to the miner for producing a valid block. Pearl follows a **polynomial-decay** schedule `R(t) = H / (t + H)` (not a discrete halving like Bitcoin, though Pearl docs still report a "blocks_to_next_halving" indicator). At height ~54k the subsidy is ~2753 PRL per block.

**Block time** — Target **194s** (≈3 min 14 s). Adjusted via WTEMA difficulty.

## C

**Cert / certificate** — A piece of a Pearl block that includes the zkSNARK proof + opening data for the matmul work. Block header is 116 bytes; cert is capped at ~65 KB.

## D

**Difficulty** — Adjusts via **WTEMA** (Weighted-Target Exponential Moving Average), τ = 7 days. Smoother than Bitcoin's 2016-block step.

## G

**Grain** — Smallest PRL unit: `10^-8 PRL` (analogous to satoshi).

## H

**Heavy-chain rule** — Pearl's tip-selection: a competing chain only wins if its work exceeds the current chain's work by more than `min(work_a, work_b) / 4`. Reduces last-block flapping.

## M

**Matrix multiplication (MatMul) PoUW** — Pearl's consensus. Miners must compute `C = A·B` over INT8 inputs (with low-rank noise matrices added for hardness), produce a tiled execution trace, and prove validity via a zkSNARK. The same matmul is the inference workload that real LLM serving requires — hence "useful work."

**Mainnet** — Pearl's primary network. RPC `44107`, P2P `44108`, wallet `44207`. Public explorer: `explorer.pearlresearch.ai`.

## O

**OP_CAT** — Re-enabled opcode in Pearl's script. Allows concatenating stack items up to 520 bytes total output. Cost: `⌈length/64⌉` against script budget. Enables some covenant-style patterns that Bitcoin disallows.

**OP_CHECKXMSSSIG** — Post-quantum signature verification opcode for **XMSS** (eXtended Merkle Signature Scheme) signatures. Novel — security model is still maturing.

**oyster** — Pearl's HD wallet daemon (forked from `btcwallet`). gRPC interface. Owns keys, balances, funds/signs/publishes transactions.

## P

**P2TR** — Pay-to-Taproot. The **only** address type in Pearl. No legacy ECDSA.

**pearld** — Pearl full node (forked from `btcd`). JSON-RPC + websocket. Owns chain, mempool, mining templates, P2P.

**pearl-gateway** — Bridge between miners and `pearld`. Caches block templates, exposes a miner-facing RPC, submits candidate blocks back to the node.

**Pearl-certified model** — A Hugging Face model in `pearl-ai/*` that has been tuned/packaged so that its inference matmuls satisfy Pearl's PoUW constraints (rank, tile shape, INT8). Currently: Llama-3.1-8B-Instruct-pearl, Llama-3.3-70B-Instruct-pearl, Gemma-4-31B-it-pearl.

**Plonky2** — The zkSNARK proof system Pearl uses to compress opening proofs from megabytes to kilobytes and preserve input privacy.

**PoUW (Proof-of-Useful-Work)** — Pearl's consensus class. Mining produces something economically valuable (LLM inference) as a by-product, instead of pure hash burn.

**`prlctl`** — Pearl's CLI/RPC client (analog of `bitcoin-cli`).

**PRL** — Pearl's native coin. Max supply **2,100,000,000 PRL**. Smallest unit: grain (10⁻⁸).

## R

**Regtest / Simnet** — Local dev networks. Simnet ports: RPC `18556`, P2P `18555`, wallet `18554`. Regtest ports: RPC `18334`, P2P `18444`, wallet `18332`. **Use these, not mainnet, for all experiments.**

## S

**SPV** — Pearl's light client. Uses compact block filters (BIP 157/158-style). Relevant for future mobile/browser wallets; not needed for backend services that can run a full `pearld`.

## T

**Taproot** — See P2TR. Pearl is Taproot-only; legacy script types not supported on-chain.

**Testnet / Testnet2** — Pearl's public test networks. Testnet RPC `44109`, P2P `44110`, wallet `44209`. Testnet2 RPC `44111`, P2P `44112`, wallet `44211`.

**Tile** — A block of the output matrix in the matmul trace. Mining proves that a randomly-selected subset of tiles was computed correctly, by opening them and verifying their hash falls below the difficulty target.

## V

**vLLM** — The high-throughput LLM serving framework Pearl miners run. `vllm-miner` is Pearl's plugin/fork.

## W

**WTEMA** — Weighted-Target Exponential Moving Average. Pearl's difficulty-adjustment algorithm. Smoother than Bitcoin's, characteristic decay τ = 7 days.

## X

**XMSS** — eXtended Merkle Signature Scheme. Stateful hash-based post-quantum signature. Available in Pearl script via `OP_CHECKXMSSSIG`. **Stateful** = a private key can only sign a bounded number of times; key management is harder than ECDSA/Schnorr. Treat as research-grade until a security review is done.

## Z

**zk-pow** — The Rust crate under `upstream/pearl/zk-pow/` implementing the Plonky2 circuit + verifier for the matmul opening proofs.

**zkSNARK** — Zero-knowledge succinct argument. Pearl uses it to (a) compress proof size, (b) hide the noise matrices that gate ASIC-resistance.
