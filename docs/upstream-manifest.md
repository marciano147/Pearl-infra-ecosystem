# Upstream Pearl Manifest

## Primary Upstream

- Repository: https://github.com/pearl-research-labs/pearl
- Inspected commit (pinned): `0c8cef72da75d10ffd52ac20d3c0b075d9d9f1f7`
- Local research checkout: `/root/.openclaw/workspace/research/pearl`

### Drift Status

Pearl upstream moves fast. The pinned commit was the inspection target when our docs were written; references to specific source paths and line ranges in `docs/` assume this commit.

Check drift before relying on internal details:

```bash
git -C upstream/pearl rev-parse HEAD                                 # what we're pinned to
git ls-remote https://github.com/pearl-research-labs/pearl HEAD      # what upstream is on
```

> **As of 2026-05-16:** upstream HEAD is `3be33a595aed2d13613d6f4ba4e1a49503513956`. We are behind. Do **not** silently `git submodule update --remote` — open a deliberate task to: (1) bump the pin, (2) re-verify every path referenced in `docs/resources.md` § "GitHub Source Map" still exists at the same location, (3) re-verify the RPC method lists in `docs/development/pearl-chain-primer.md` §5–§6, and (4) record the new commit + diff summary here.


## External Resource Index

The complete link/API map lives in `docs/resources.md`. Keep that file updated whenever a new Pearl source, endpoint, model, or app reference is discovered.

Key external roots:

- https://pearlresearch.ai/
- https://github.com/pearl-research-labs/pearl
- https://huggingface.co/pearl-ai
- https://pearl-otc.com/
- https://api.pearl-otc.com
- https://compute.pearlresearch.ai/

## Relevant Components

| Path | Purpose | Initial Handling |
|---|---|---|
| `README.md` | Top-level architecture and node/miner flow | Reference/copy excerpts only |
| `node/` | `pearld` full node and JSON-RPC | Adapter target; no fork yet |
| `node/docs/json_rpc_api.md` | Chain RPC reference | Reference for typed adapter |
| `wallet/` | `oyster` HD wallet daemon | Adapter target; no fork yet |
| `wallet/rpc/documentation/api.md` | Wallet gRPC/API reference | Reference for wallet SDK |
| `spv/` | Pearl light client | Research for mobile/browser future |
| `apps/apps/pearl-desktop-wallet/` | Electron desktop wallet | Reference UX/RPC patterns |
| `apps/packages/pearl-address-validation/` | Pearl address validation package | Candidate for SDK wrap/extract |
| `miner/pearl-gateway/` | Node/miner gateway | Compute marketplace reference |
| `miner/vllm-miner/` | vLLM mining/inference stack | Compute marketplace reference |
| `py-pearl-mining/` | Python mining bindings | Compute integration reference |
| `zk-pow/` | Proof system | Research only |

## Handling Rules

- Prefer submodule or fetch script before copying upstream source.
- Copy only minimal license-compatible docs/types/examples required for stable development.
- If KaspaCom modifies upstream internals, create a deliberate fork plan and document merge/update process.
- Never commit generated wallets, seeds, private keys, RPC passwords, or `.env` files.
