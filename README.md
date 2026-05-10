# pearl-infra

KaspaCom infrastructure foundation for building Pearl ecosystem apps.

## Purpose

Pearl is a Bitcoin-style UTXO L1 with a working node (`pearld`), wallet daemon (`oyster`), desktop wallet, SPV/light client, JSON-RPC/wallet RPC surfaces, Taproot/P2TR addresses, OP_RETURN/null-data support, Blockbook-style endpoints, OTC market APIs, and Pearl-certified Hugging Face/vLLM models.

This repo is the planning and infrastructure base for KaspaCom Pearl apps:

- chain/node adapters
- wallet SDK primitives
- app-facing chain data APIs
- Pearl Pay/payment rails
- OTC/market-data rails
- AI compute marketplace control-plane interfaces
- future browser/mobile wallet connector interfaces


## Fast Research Links

Start here before any Pearl task:

- Resource hub: `docs/resources.md`
- Developer local setup: `docs/development/local-dev-guide.md`
- Pearl chain primer: `docs/development/pearl-chain-primer.md`
- Pearl app development guide: `docs/development/pearl-app-development.md`
- Next steps: `docs/next-steps.md`
- Upstream manifest: `docs/upstream-manifest.md`
- App thesis: `docs/research/pearl-app-thesis.md`
- OpenSpec: `docs/openspec/pearl-infra-ecosystem/`

## Current Phase

**Planning/bootstrap only.** No production wallet, explorer, payment, or marketplace code should be shipped before the OpenSpec change is reviewed and approved.

OpenSpec: `docs/openspec/pearl-infra-ecosystem/`

## Non-Goals

- Do not build a browser extension before there is a real web app/use case requiring wallet signatures.
- Do not assume EVM-style smart contracts, DeFi, or token mechanics unless Pearl support is verified.
- Do not fork/copy the full Pearl monorepo blindly.
- Do not commit secrets, wallet seeds, private keys, RPC passwords, or mainnet custody material.

## Planned Layout

```text
apps/       Thin prototype UIs, if needed later
packages/   Reusable SDKs and typed adapters
services/   Chain data API, payment, market, compute control-plane services
ops/        Devnet, Docker, deployment, observability, runbooks
docs/       Specs, research, upstream manifest, architecture notes
```

## Goal-Based Execution

Every implementation task must include:

1. success criteria
2. applicable skill(s)
3. verification command(s)
4. failure escalation path

A task is not complete until its deliverable exists and verification evidence is recorded.


## Upstream Source

Pinned upstream Pearl source is linked as a submodule:

```bash
git submodule update --init --recursive
```

Path: `upstream/pearl`
Commit: `0c8cef72da75d10ffd52ac20d3c0b075d9d9f1f7`
