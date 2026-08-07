# Code Radius — demo & share prompts

Copy-paste these to try the plugin, record a demo, or post socially.

## 30-second demo

```text
Map the Code Radius for adding rate limiting to the login endpoint.
Don’t edit yet—show entry points, sibling surfaces, tests, risks,
and a bounded edit set.
```

After the report:

```text
Go ahead—only touch the edit set. Then run radius-verify.
```

## More scenarios

**Rename / move**

```text
Code Radius: rename `UserService` to `AccountService` in the API package.
Don’t edit until I see callers, tests, and anything out of scope.
```

**Migration**

```text
Map the Code Radius for adding a non-null `plan_id` to the accounts table.
Include app code, jobs, and rollback risk. Don’t migrate yet.
```

**Bugfix with unknown fan-out**

```text
We’re seeing duplicate charges on retry. Run Code Radius on the payment
capture path before changing anything.
```

**Auth**

```text
Impact radius for requiring verified email before session issue.
Leave OAuth alone unless it’s truly in scope.
```

**Pre-PR check**

```text
Run radius-verify on my current diff against the last Code Radius edit set.
```

## Social one-liners

- Code Radius: don’t let your coding agent edit until it maps what the change will touch.
- Impact report → bounded edit set → implement → verify. Portable Agent Plugin, no API keys.
- Stop merge surprises. Run Code Radius before the first keystroke of a risky change.

## Hashtags / discovery terms

`#AgentPlugins` `#Cursor` `#Codex` `#AICoding` `#CodeRadius` `impact analysis` `edit set` `safe refactor`
