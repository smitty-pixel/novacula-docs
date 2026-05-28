# Glossary

Canonical terminology for the Novacula docs. Use these terms consistently; don't introduce synonyms.

| Term | Meaning |
| --- | --- |
| node | A blockchain full node managed by Novacula — one logical unit in the UI, one or more processes on the executor side |
| process | A single OS service or container belonging to a node (e.g. Ethereum: EL process + CL process) |
| full node | Default node type — keeps recent state; serves the standard RPC surface |
| archive node | A node retaining full historical state (Ethereum, BSC); larger disk, slower initial sync, supports debug/trace RPCs at any block height |
| control plane | The Novacula SaaS service (or its self-hosted equivalent) that stores desired-state specs and serves the UI / GraphQL API |
| executor | A Novacula agent or operator instance running in your infrastructure that polls the control plane and reconciles node specs |
| agent | Bare-metal executor backend; manages nodes as `systemd` services on a Linux host |
| operator | Kubernetes executor backend; manages nodes as a `BlockchainNode` CR → `ConfigMap` + `StatefulSet` |
| chain adapter | Per-chain implementation of the `ChainAdapter` interface (config generation, install, init steps, port plan, metrics) |
| reconciler | Loop inside an executor that diffs `desired` vs `observed` and applies changes |
| desired state | The target spec the control plane stores for a node; what the reconciler converges to |
| observed state | The latest state report the executor pushes back for a node |
| revision | Monotonically increasing version on a node's spec; used to detect drift between desired and observed |
| capabilities | The set of chains, networks, clients, versions, and node types an executor declares it can run, sent on every sync |
| snapshot | An external pre-synced data directory (e.g. Tron `/backup<date>/`) used to bootstrap a node faster than syncing from genesis |
| organization | Tenant boundary in the platform (better-auth concept); everything is scoped to one |
| API key | Org-scoped credential an executor or external script presents to authenticate with the control plane |
| session | Browser/IDE login session tied to a user and an active organization |
| audit log | Append-only who-did-what record carrying actor + typed action + target — see `/docs/audit/audit-log` |
| events feed | Resource-lifecycle stream from control plane + executors; events do not require an actor — see `/docs/notifications/events-feed` |
| incident | Lifecycle-tracked alert (status `open` → `resolved`) opened by an alert rule and optionally delivered via webhook |
| assistant thread | A persisted conversation in the AI Assistant chat panel; the active thread id is restored across navigation |

## Don't use these terms for Novacula

- **Validator node / validator** — Novacula manages blockchain nodes, not validators. "Validator" is fine when referring to external chain entities (e.g. Sui validator-network P2P seeds, "point your validator client at the beacon API") but never to describe what the platform itself does.

## Keep in English everywhere

- Product, component, and service names: `Novacula`, `control plane`, `executor`, `reconciler`, `agent`, `operator`
- Technology names: `Bun`, `NestJS`, `Prisma`, `libSQL`, `GraphQL`, `Apollo`, `Kubernetes`, `Helm`, `systemd`, `Docker`, `Vite`, `React`, `TanStack Router`, `Tailwind`, `CASL`, `better-auth`, `Biome`
- Code identifiers, API fields, config keys, environment variables, CLI commands, URLs, file paths
