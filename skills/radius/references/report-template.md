# Impact Radius Report (template)

Brand this output as **Code Radius**. Copy the structure, delete empty sections, cite as `path` or `path:line`.

```markdown
## Code Radius: <short change title>

**Change:** <one sentence>
**Success:** <what “done” means>
**Must not break:** <invariants>

### Entry points
- …

### Also hits
- … — why it is in radius

### Shared / core
- …

### Contracts & config
- types / schemas / env / events / infra: …

### Tests
- covered: …
- gaps: …

### Risks
- [high] …
- [medium] …
- [low] …

### Edit set (in scope)
1. `path` — intended change
2. …

### Leave alone (out of scope)
- `path` — reason

### Assumptions
- …

### Open questions
- … (omit if none)

**Ready to implement?** <waiting for go | proceeding as requested>
```

## Example (filled) — share this as a demo

```markdown
## Code Radius: rate-limit login

**Change:** Add application-level rate limiting to password login.
**Success:** Repeated failed logins from one actor are throttled; legitimate users still log in.
**Must not break:** OAuth login, mobile refresh, admin impersonation.

### Entry points
- `POST /auth/login` → `src/routes/auth.ts:88` (`loginHandler`)

### Also hits
- `src/auth/oauth-callback.ts:40` — shares `issueSession` after identity proof
- `src/cli/login.ts:12` — CLI password login calls same handler stack

### Shared / core
- `src/auth/session.ts` — session issuance
- `src/lib/redis.ts` — candidate store for counters

### Contracts & config
- Gateway: Kong route `/auth/*` already has 100/min IP limit (`infra/kong/auth.yaml:22`)
- Env: no existing `LOGIN_RATE_*` vars found

### Tests
- covered: `tests/auth/login.test.ts`
- gaps: no CLI login tests; mobile OAuth covered in `tests/oauth/mobile.test.ts` (should remain green)

### Risks
- [high] Double limit (gateway IP + app) may lock out NAT users
- [medium] Mobile refresh-then-login pattern may trip naive IP limits
- [low] CLI from CI shared IPs

### Edit set (in scope)
1. `src/routes/auth.ts` — attach limiter to password login only
2. `src/auth/rate-limit.ts` — new helper (create)
3. `tests/auth/login.test.ts` — throttle cases
4. `src/config/env.ts` — `LOGIN_RATE_MAX` / `LOGIN_RATE_WINDOW_SEC` if needed

### Leave alone (out of scope)
- `infra/kong/auth.yaml` — keep gateway limit until product decides
- `src/auth/oauth-callback.ts` — OAuth not in this change

### Assumptions
- Product wants password-login limiting first, not all `/auth/*`

### Open questions
- Limit by IP, account, or both?

**Ready to implement?** waiting for go
```
