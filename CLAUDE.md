# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

User-facing documentation for **Novacula** — a B2B platform for managing blockchain nodes. Built with [Mintlify](https://mintlify.com).

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
en/            ← master version (source of truth)
ru/            ← localized version (Russian)
docs.json      ← Mintlify config + multi-language navigation
glossary.md    ← unified terminology across locales
```

## Product context

Novacula is a B2B platform for managing blockchain validator nodes. Key concepts the docs cover:

- **Control plane**: NestJS + Apollo GraphQL service; desired-state model, pushes specs with revisions.
- **Two execution backends** share the same `ChainAdapter` interface:
  - **Agent** — bare-metal daemon managing nodes as systemd services.
  - **Operator** — Kubernetes operator (CRD watcher → ConfigMap + StatefulSet).
- **Outbound-only**: executors initiate HTTP polling connections to the control plane.
- One node = N processes (e.g. Ethereum: EL + CL). Each process = a separate OS service or container.

## Localization rules

- `en/` is the source of truth.
- Every page in `en/` must have a matching page in `ru/`.
- Keep file paths identical across locales (`en/foo/bar.mdx` ↔ `ru/foo/bar.mdx`).
- Translate titles, descriptions, headings, body text, callouts, and UI explanations.
- Do **not** translate code, API fields, URLs, config keys, environment variables, or command names.
- Preserve Mintlify MDX components and their structure (`<Card>`, `<CodeGroup>`, `<Tabs>`, `<Accordion>`, etc.).
- Update `docs.json` navigation for **all** supported languages whenever pages are added, removed, or renamed.
- Use [glossary.md](glossary.md) as the single source of truth for terminology. Do not introduce synonyms.

## Workflow for adding a page

1. Author the page in `en/` (source of truth).
2. Create the matching file at the same path in `ru/`.
3. Translate per the rules above; check [glossary.md](glossary.md) for canonical terms.
4. Add the page under the corresponding language group in `docs.json`.

## Self-Updating Rules

If guidance in this file is incomplete, outdated, or contradicted by real practice — **update it in the same change**. When a new convention emerges (new MDX pattern, new glossary term, new locale), add it here or to [glossary.md](glossary.md).

## Personal Rules

If `.claude/personal-rules/` directory exists, load and follow all `.md` files in it.
