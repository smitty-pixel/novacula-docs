# Novacula documentation

Public user documentation for **Novacula** — a blockchain node infrastructure platform for running your own full nodes for RPC (JSON-RPC, WebSocket, gRPC) on bare-metal or Kubernetes you control, with no third-party gateway between your apps and your nodes.

Built with [Mintlify](https://mintlify.com). English only.

## Quick start

Preview the site locally with hot reload:

```bash
bun install -g mint   # one-time install of the `mint` CLI
mint dev              # http://localhost:3000
```

Without a global install: `bunx mint dev`.

Requires **Bun ≥ 1.3.0**. Run from the repo root (where `docs.json` lives). Use `mint dev` to verify Mintlify MDX components (`<Card>`, `<CodeGroup>`, `<Tabs>`, ...) render as intended — editor markdown previews don't render these.

## Structure

```
docs/              Cloud > Guides (get-started, platform, account, executors,
                   nodes, notifications, chains; plus introduction.mdx)
recipes/           Cloud > Recipes (infrastructure how-tos)
reference/         Cloud > API (graphql-api, configuration, env-vars, agent-cli)
release-notes/     Cloud > Release notes
self-hosted/       Self-hosted deployment mode (single coming-soon page today)
docs.json          Mintlify config: two top-level dropdowns (Cloud / Self-hosted)
glossary.md        Canonical terminology
CLAUDE.md          Project guidance loaded by AI coding agents
```

The rendered site has a top-of-page deployment-mode dropdown — **Cloud** (managed) vs **Self-hosted** — wired via `navigation.dropdowns` in `docs.json`.

## What Novacula actually is

A B2B platform for operating blockchain full nodes for RPC at scale — the kind teams run to serve their own apps, indexers, exchanges, or internal data tooling instead of depending on a public RPC provider. Two execution backends share the same `ChainAdapter` interface:

- **Agent** — bare-metal daemon, manages node processes as `systemd` services.
- **Operator** — Kubernetes controller, materializes nodes as `ConfigMap` + `StatefulSet`.

The control plane is desired-state: it stores specs with revisions; executors poll outbound and reconcile. Nothing in your network needs to accept inbound connections. The platform manages the blockchain node (e.g. Ethereum's EL + CL pair) — it does **not** run a validator client.

Two deployment modes, both keeping the blockchain processes on hardware you own:

- **Cloud** — Novacula operates the control plane; you bring the node hardware.
- **Self-hosted** — the entire stack lives inside your network (currently coming soon; see [`self-hosted/introduction.mdx`](self-hosted/introduction.mdx)).

## Adding or editing a page

1. Author the page under the right subtree:
   - Cloud content → `docs/`, `recipes/`, `reference/`, or `release-notes/`
   - Self-hosted content → `self-hosted/`
2. Add the page to the appropriate group in [`docs.json`](docs.json) (inside the right `dropdown`).
3. Cross-link from related pages with root-relative paths (`/docs/...`, `/reference/...`).
4. Run `mint dev` and visually verify the rendered page.

## Conventions

- **Internal links** are root-relative, no language or version prefix: `/docs/executors/overview`, `/reference/graphql-api`, `/self-hosted/introduction`.
- **Terminology** — use canonical terms from [`glossary.md`](glossary.md); don't introduce synonyms.
- **Don't translate** code, API fields, URLs, config keys, environment variables, or command names — even inside prose.
- **Don't position Novacula as "validator management"** — it manages blockchain nodes (EL+CL pair, `bitcoind`, etc.), not validator clients. `validator` is OK only when referring to external chain entities (e.g. Sui validator-network P2P seeds, an Ethereum CL `validator count` metric). See [`glossary.md`](glossary.md#dont-use-these-terms-for-novacula).
- **MDX components** — preserve the Mintlify primitives (`<Card>`, `<CodeGroup>`, `<Tabs>`, `<Accordion>`, `<Note>`, `<Warning>`, ...). Plain markdown previews don't render them; always check with `mint dev`.

## Repository

Canonical remote: [`RSquad/novacula-docs`](https://github.com/RSquad/novacula-docs). Default branch is `master`.

## Working with AI agents

[`CLAUDE.md`](CLAUDE.md) carries detailed guidance for AI coding agents (Claude Code, etc.) — product context, repo conventions, anti-patterns to avoid. If you run an agent against this repo, it will pick that file up automatically.
