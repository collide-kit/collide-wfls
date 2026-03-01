# AGENTS.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo is

`@collide-kit/collide-wfls` is a collection of GitHub Actions composite actions for CI/CD in Collide Tech projects. There is **no build step, no TypeScript, no tests to run locally** — this is a pure YAML/shell repository. All testing happens via GitHub Actions on push/PR.

## Commands

```bash
yarn fmt:check        # Check formatting (runs in CI)
yarn fmt:fix          # Fix formatting issues
```

Use `yarn fmt:fix:cache` / `yarn fmt:check:cache` for faster repeated runs locally.

## Architecture

The repo has a flat structure under `actions/`:

- **`actions/prepare/`** — The primary composite action. Orchestrates setup-node + setup-yarn + cache-deps + yarn install in one step. This is what consumers reference as `collide-kit/collide-wfls/actions/prepare@v1`.
- **`actions/setup-node/`** — Thin wrapper around `actions/setup-node` with a verification step.
- **`actions/setup-yarn/`** — Enables Corepack, installs Yarn at the specified version, exposes version and cache-dir outputs.
- **`actions/cache-deps/`** — Two-level cache (`yarn cache` + `node_modules`) keyed on `{os}-node-{version}-yarn-{version}-{hash(yarn.lock, package.json)}`.
- **`actions/release/`** — Empty directory (reserved).

CI workflows in `.github/workflows/`:

- `ci.yml` — Orchestrator that calls `format.yml` and `test.yml` as reusable workflows, with a final `ci-ok` gate job.
- `format.yml` — Runs `yarn fmt:check` using the `./actions/prepare` composite action itself (self-referential test).
- `test.yml` — Comprehensive matrix tests (Ubuntu/macOS/Windows × Node 20/22/24/latest) covering default, no-cache, no-install, custom-version, and cache-hit scenarios.

## Conventions

- All third-party action references must be **pinned to a full SHA** (e.g., `actions/setup-node@6044e13b5dc448c55e2357c09f80417699197238 # v6.2.0`). Never use mutable tags like `v4` or `main`.
- Yarn Berry (v4) via Corepack is the package manager. Run `yarn install` (not `npm install`) to update `node_modules`.
- Prettier config lives in `package.json`. YAML/JSON/Markdown use 2-space indent; JS/TS files use tabs. The `@prettier/plugin-oxc` plugin handles JS/TS formatting.
- `yarn install --immutable` is enforced in CI. If `yarn.lock` is out of sync, run `yarn install` locally and commit the updated lockfile.
