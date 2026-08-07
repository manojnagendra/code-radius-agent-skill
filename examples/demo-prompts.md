# Code Radius — demo & share prompts

Works in any **Agent Plugins** client that loads this package’s skills. Copy-paste to try, record a demo, or post socially.

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

- Code Radius: an Agent Plugins skill pack—don’t edit until the impact radius is mapped.
- Impact report → bounded edit set → implement → verify. Open standard, no API keys.
- Portable across Agent Plugins clients. One `plugin.json`, three skills.

## Discovery terms

`#AgentPlugins` `#AgentSkills` `#CodeRadius` `impact analysis` `edit set` `safe refactor`
