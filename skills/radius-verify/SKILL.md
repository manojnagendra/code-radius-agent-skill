---
name: radius-verify
description: "Code Radius verify — check that a completed or in-progress diff stayed inside the agreed impact-radius edit set. Use after implementing a change mapped with Code Radius, before opening a PR, or when the user asks to verify edit set, check scope drift, confirm we only touched the radius, or audit out-of-scope files. Reports in-scope changes, out-of-set files, missing tests from the report, and a verdict (clean / extend-radius / revert-out-of-set)."
license: MIT
metadata:
  author: Code Radius
  version: "1.0.0"
  plugin: code-radius
---

# Code Radius verify

After implementation (or when asked), compare the actual diff to the last **Impact Radius Report**.

## Steps

1. Recover the agreed **edit set** and **leave alone** list from the conversation (or ask if missing).
2. Inspect the current change (`git status` / `git diff` when available; otherwise list files you edited).
3. Classify every touched file:

| Class | Meaning |
| ----- | ------- |
| **in-set** | Listed in the edit set |
| **new-in-radius** | Not listed, but clearly required by the same change (report should have been updated) |
| **out-of-set** | Unrelated or previously “leave alone” |
| **missing** | In the edit set but never changed (explain if the plan evolved) |

4. Check that **tests** called out in the report were added or updated, or explain why not.
5. Emit a short verification report:

```markdown
## Code Radius verify

**In set:** …
**New (should have extended radius):** …
**Out of set:** …
**Edit set not touched:** …
**Tests:** …

**Verdict:** clean | extend-radius | revert-out-of-set
```

## Actions

- **clean** — proceed to PR/summary
- **extend-radius** — update the impact report and justify new files
- **revert-out-of-set** — recommend reverting or splitting unrelated edits

Ignore harmless lockfile or generated-file noise; mention once under “noise” if needed.
