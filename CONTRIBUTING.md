# Contributing

This repo is OpenSpec-first. Do not jump into product code without a scoped task and verification evidence.

## Workflow

1. Read `README.md`, `docs/resources.md`, and `docs/upstream-manifest.md`.
2. Check `docs/openspec/pearl-infra-ecosystem/tasks.md`.
3. Pick one small task.
4. Record success criteria before implementation.
5. Implement.
6. Verify with the smallest meaningful command/test.
7. Update docs/tasks with evidence.
8. Commit cleanly.

## Goal-Based Execution Checklist

Every implementation task must include:

- **Success criteria** — what artifact/behavior proves the task is done.
- **Applicable skill(s)** — e.g. GitHub, TypeScript, NodeJS, browser UX, code-quality gates.
- **Verification** — command, test, build, API call, or inspection evidence.
- **Failure escalation** — if a step fails twice, stop and report exact error + attempted fix.

## Security Rules

- Never commit wallet seeds, private keys, RPC passwords, `.env`, API tokens, or logs containing secrets.
- Use testnet/regtest fixtures until mainnet operations are explicitly approved.
- Any escrow/multisig/custody feature requires security review before mainnet use.

## Upstream Pearl

Pearl upstream is linked as a pinned submodule at `upstream/pearl`.

```bash
git submodule update --init --recursive
```

Do not modify submodule code casually. If KaspaCom needs upstream changes, create a deliberate fork/update plan first.
