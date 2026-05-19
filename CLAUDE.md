# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

User-facing documentation for **Novacula** — a B2B platform for managing blockchain nodes. Built with [Mintlify](https://mintlify.com). English only.

## Runtime

**Always use Bun** — not npm, not yarn, not pnpm. Bun is the package manager and CLI runner. Minimum version: **1.3.0** (enforced via `engines` in `package.json` where present).

## Local preview

Install the Mintlify CLI globally via Bun and run the dev server from the repo root (where `docs.json` lives):

```bash
bun install -g mint    # one-time, installs the `mint` CLI
mint dev               # local preview at http://localhost:3000, with hot reload
```

Alternatively, run without a global install: `bunx mint dev`.

Use `mint dev` to verify Mintlify MDX components (`<Card>`, `<CodeGroup>`, `<Tabs>`, ...) render as intended — markdown previews in editors do not render these.

## Structure

```
docs/              ← Cloud > Guides (get-started, platform, account, executors,
                     nodes, notifications, chains, plus introduction.mdx)
recipes/           ← Cloud > Recipes (infrastructure how-tos)
reference/         ← Cloud > API (graphql-api, configuration, env-vars, agent-cli)
release-notes/     ← Cloud > Release notes
self-hosted/       ← Self-hosted deployment mode (currently a single
                     coming-soon introduction page)
docs.json          ← Mintlify config: two top-level dropdowns (Cloud / Self-hosted)
glossary.md        ← canonical terminology
```

The navigation switcher in the rendered docs is a **deployment-mode dropdown** (Cloud / Self-hosted), implemented via `navigation.dropdowns` in [docs.json](docs.json). Page paths follow `<tab-dir>/<group>/<page>` (e.g. `docs/executors/overview`, `self-hosted/introduction`).

## Product context

Novacula is a B2B platform for operating **blockchain full nodes for RPC** — JSON-RPC, WebSocket, and gRPC endpoints — on your own infrastructure, at scale. Customers are teams that need to run nodes themselves rather than depend on a public RPC provider: app teams, indexers, exchanges, MEV ops, internal data platforms. The platform does **not** run validator clients or consensus signing; it manages the underlying node (e.g. Geth + Lighthouse pair for Ethereum) that a validator could be pointed at separately.

Key concepts the docs cover:

- **Control plane**: NestJS + Apollo GraphQL service; desired-state model, pushes specs with revisions.
- **Two execution backends** share the same `ChainAdapter` interface:
  - **Agent** — bare-metal daemon managing nodes as systemd services.
  - **Operator** — Kubernetes operator (CRD watcher → ConfigMap + StatefulSet).
- **Outbound-only**: executors initiate HTTP polling connections to the control plane.
- One node = N processes (e.g. Ethereum: EL + CL). Each process = a separate OS service or container.
- **Two deployment modes**: **Cloud** (control plane operated by Novacula) and **Self-hosted** (full stack inside the customer's network). Cloud is fully documented; self-hosted is "coming soon".

When writing positioning copy, **don't use "validator" to describe Novacula's purpose** — the platform manages nodes, not validators. "Validator" is fine when referring to external chain entities (e.g. Sui validator-network P2P seeds, Ethereum CL `validator count` metric, "point your validator client at the beacon API").

## Conventions

- Internal links are root-relative without language prefix: `/docs/executors/overview`, `/reference/graphql-api`, `/self-hosted/introduction`.
- Use canonical terms from [glossary.md](glossary.md); don't introduce synonyms.
- Don't translate code, API fields, URLs, config keys, environment variables, or command names — even in narrative prose.
- Preserve Mintlify MDX components (`<Card>`, `<CodeGroup>`, `<Tabs>`, `<Accordion>`, `<Note>`, `<Warning>`, ...) and their structure.

## Workflow for adding a page

1. Author the page under the right subtree: Cloud content under `docs/` / `recipes/` / `reference/` / `release-notes/`, self-hosted content under `self-hosted/`.
2. Add the page to the appropriate group in [docs.json](docs.json) (inside the right `dropdown`).
3. Cross-link from related pages; use root-relative paths (`/docs/...`).

## Self-Updating Rules

If guidance in this file is incomplete, outdated, or contradicted by real practice — **update it in the same change**. When a new convention emerges (new MDX pattern, new glossary term, new dropdown), add it here or to [glossary.md](glossary.md).

## Personal Rules

If `.claude/personal-rules/` directory exists, load and follow all `.md` files in it.
