# Risk catalog

Use these labels in the Impact Radius Report. Prefer evidence over vibes.

## Severity

| Level | Meaning |
| ----- | ------- |
| **high** | Likely user-visible outage, data loss, security hole, or money wrong |
| **medium** | Partial breakage, flaky paths, missing tests on a hot path |
| **low** | Edge cases, docs drift, cosmetic inconsistency |

## Common patterns to check

### Double application
The same control already exists at another layer (API gateway, CDN, middleware, library default). Changing both can over-restrict or conflict.

### Shared helper fan-out
Editing a utility used by web + mobile + CLI + jobs. Narrow the change to one caller when possible; otherwise expand the edit set and tests.

### Contract drift
Types, OpenAPI, protobufs, event schemas, or env vars disagree with runtime code. Call it out; do not “fix” one side silently unless that is the task.

### Test hollow
Behavior has unit tests but no integration/e2e on the real entry point—or the opposite. List gaps next to covered tests.

### Hot paths
Auth, session, payments, permissions, migrations, multi-tenant filters, crypto. Raise severity when these are in radius.

### Implicit ordering
Init order, migrations that must run before deploy, feature flags that gate half the path. Note deploy/order constraints in risks.

### Name collisions
Multiple `login`, `User`, or `client` symbols. Cite the specific one in the edit set so implementation does not wander.

### Infra twin
Cron, queues, IaC, or CI still assume old paths/rates/env. Include them in “also hits” or “leave alone” with a reason.

## What not to do

- Do not inflate risk to sound dramatic
- Do not list the whole monorepo as high risk
- Do not skip citations on high/medium items
