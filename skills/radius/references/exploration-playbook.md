# Exploration playbook

Task-scoped recon, not a full codebase tour. Stop when the edit set is stable.

## Phase A — Orient (minutes, not hours)

1. Read root manifests: `README*`, `package.json` / `pyproject.toml` / `go.mod` / `Cargo.toml`, workspace config.
2. Detect archetype: web app, API, CLI, library, monorepo package, infra, data pipeline, mobile.
3. Identify the package or service that owns the change (monorepos: do not map every package).

## Phase B — Pin the entry

Search in this order:

1. User-given file/symbol/URL/error
2. Route / command registrations
3. Test names that describe the behavior
4. Types/schemas named like the feature

Record **one primary entry** and only then expand.

## Phase C — Expand one hop at a time

For each symbol in the current set:

1. Find definitions
2. Find references / importers
3. Find tests referencing the symbol or route
4. Find config/infra strings (paths, env keys, queue names)

Add a file to the radius only if changing the feature **requires** touching it or it will **observe** the change (callers, contracts, tests).

## Phase D — Archetype hints

### HTTP API
Routers → handlers → services → repos; gateway/CDN; OpenAPI; auth middleware order.

### Frontend / full-stack
Page/route → data hooks → API client → server actions; feature flags; analytics events.

### CLI
Command registration → flags → shared lib; exit codes; CI invocations of the command.

### Library / SDK
Public exports → internal modules → downstream packages in the same repo; semver-sensitive surfaces.

### Data / jobs
Schedule/trigger → worker → store; idempotency keys; migration pairing.

## Phase E — Keystone verify

Before publishing the report, re-read the citations for:

- The primary entry
- Any **high** risk claim
- Any file in the edit set you have not opened yet

If a citation was wrong, fix the report—do not carry forward a bad map.

## Anti-patterns

- Dumping `tree` / giant directory lists
- “Architecture overview” unrelated to the change
- Marking half the repo in-scope “just in case”
- Editing while still searching
