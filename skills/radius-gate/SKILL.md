---
name: radius-gate
description: "Code Radius gate — enforce impact-radius recon before code edits. Use when about to modify files without an Impact Radius Report, when scope is expanding past the agreed edit set, or when the user says don't edit yet, run radius first, code radius first, gate this change, preflight first, or stop and map impact. Blocks implementation until Code Radius completes or the user explicitly overrides (skip radius / just do it)."
license: MIT
metadata:
  author: Code Radius
  version: "1.0.0"
  plugin: code-radius
---

# Code Radius gate

Stop implementation and run (or refresh) **radius** when any of these are true:

- No Impact Radius Report exists for the current task
- The planned edit set is empty or “TBD”
- New files/symbols appeared that were not in the last report
- The user said to wait, gate, preflight, or map impact first

## Behavior

1. **Do not** write/create/delete code files yet (discussion and read-only search are fine).
2. Say briefly that you are gating on Code Radius (impact mapping).
3. Run the `radius` skill workflow and publish the report.
4. Resume edits only after:
   - the user approves, or
   - the user already asked to proceed after radius, or
   - the user explicitly overrides (“skip radius”, “just do it”).

## Override

If the user overrides, acknowledge once, skip the gate, and continue. Do not nag.

## Scope creep

If during implementation you discover new affected surfaces:

1. Pause edits
2. Update the Impact Radius Report (a short delta is fine)
3. Ask to extend the edit set before touching new files
