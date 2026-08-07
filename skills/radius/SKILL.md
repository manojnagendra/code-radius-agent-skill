---
name: radius
description: "Code Radius — map the impact radius of a proposed code change before editing. Use for features, bugfixes, refactors, renames, migrations, API/schema changes, auth, payments, permissions, shared utilities, or when the user says code radius, map the radius, impact radius, change scope, affected surface, what will this touch, what breaks, don't edit yet, bound the edit set, preflight this change, or scope this PR. Also use when about to modify code with unclear callers or cross-module risk. Produces a cited Impact Radius Report (entries, callers, shared core, contracts, tests, infra, risks) and a bounded edit set. Do not implement until the report is delivered unless the user explicitly skips Code Radius."
license: MIT
metadata:
  author: Code Radius
  version: "1.0.0"
  plugin: code-radius
---

# Code Radius — impact mapping before edits

**Tagline:** Don’t edit until you know what the change will touch.

Do **not** start implementing a non-trivial change until you have produced an **Impact Radius Report** and a **bounded edit set**. Trivial typos and single-line local fixes may skip this; when unsure, run Code Radius.

Prefer user-facing terms: **impact radius**, **affected surface**, **edit set**, **downstream callers**, **risk**. Avoid violent metaphors.

## Why this exists

AI agents often grep once, read a few files, and edit. They miss sibling entry points, shared helpers, gateway twin configs, and test gaps. You only see the real impact in review or production. Code Radius makes **task-scoped impact recon** mandatory: cite the surface, bound the edit set, then change only that set.

## When to activate

Activate for changes that may cross module boundaries, including:

- New behavior on existing endpoints, jobs, CLI commands, or UI flows
- Renames, moves, API / schema / event contract changes
- Auth, billing, permissions, multi-tenant filters, migrations
- Shared utilities with unknown fan-out
- Anything touching more than one file or unknown call sites
- User asks to preflight, scope, or “what will this touch?”

Skip only when the user explicitly says to skip Code Radius / radius, or the change is clearly isolated (e.g. fix a typo in a comment).

## Hard rules

1. **Recon before write** — no code edits until the report is shown (or the user opts out).
2. **Cite everything** — every claim needs `path` or `path:line` (or an honest “unverified”).
3. **Separate fact from guess** — label assumptions; re-read keystone citations before publishing the report.
4. **Bound the edit set** — list files in-scope and explicitly out-of-scope.
5. **Ask only when blocked** — if a product/infra choice changes the radius, ask once with options; otherwise deliver the report.
6. **Stay in the set** — after approval, implement only inside the edit set; if new impact appears, pause and extend the report.

## Workflow

### 1. Restate the change

One sentence: intent, success criteria, and what must not break.

### 2. Find entry points

Locate the primary symbols, routes, jobs, CLI commands, or UI surfaces involved. Prefer exact symbol/route search over vague directory walks. Use manifests, routers, OpenAPI, and tests that name the behavior.

Read [references/exploration-playbook.md](references/exploration-playbook.md) when the stack is unclear.

### 3. Expand the radius

From each entry, map:

| Layer | Look for |
| ----- | -------- |
| Callers / importers | Who invokes this symbol or depends on this module? |
| Callees / shared core | Shared services, DB, cache, queues, feature flags |
| Contracts | Types, schemas, OpenAPI, events, env vars |
| Tests | Unit, integration, e2e covering the path |
| Infra / config | Gateways, cron, IAM, Terraform, CI assumptions |
| Sibling surfaces | Same helper used by mobile, CLI, admin, webhooks |

Stop when more hops are unrelated to the stated change. Prefer a **tight** edit set over a repo tour.

### 4. Score risk

Use [references/risk-catalog.md](references/risk-catalog.md). Call out double application (e.g. limit already at the gateway), missing coverage, and hot paths (auth, money, migrations).

### 5. Emit the report

Use [references/report-template.md](references/report-template.md). Keep it short enough to act on—not an architecture essay.

### 6. Gate implementation

End with:

- **Edit set** (files to change)
- **Leave alone** (out of scope, with reasons)
- **Open questions** (only if they change the plan)
- **Ready to implement?** waiting for user “go” unless they already said to proceed after radius

## Output quality bar

- Prefer ~1 page over a codebase dump
- No filler folder trees
- Every “also hits” / “shared” / “tests” line cited or marked unverified
- If callers are missing, say what you searched

## Demo prompt (shareable)

> Map the Code Radius for adding rate limiting to login. Don’t edit yet—show entry points, sibling surfaces, tests, risks, and a bounded edit set.

## Related skills in this plugin

- `radius-gate` — enforce no-edit-until-mapped mid-task
- `radius-verify` — after edits, confirm the diff stayed inside the edit set
