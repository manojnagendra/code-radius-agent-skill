# Code Radius

**Don’t edit until you know what the change will touch.**

Code Radius is an [Agent Plugin](https://agent-plugins.org/) that teaches AI coding agents to run **task-scoped impact analysis** before they write code. Install once. Works across clients that support Agent Plugins—Cursor, ChatGPT/Codex, GitHub Copilot, VS Code, Kiro, and more.

No API keys. No hosted MCP. Pure portable [Agent Skills](https://agentskills.io/).

---

## The problem (why people install this)

Agents are great at *finding* files and bad at *scoping* a change.

Typical failure:

1. You ask: “Add rate limiting to login.”
2. The agent finds one route handler, edits it, and declares victory.
3. It missed mobile OAuth sharing the same session helper, a CLI login path, a gateway that already throttles `/auth/*`, and tests that never covered the CLI.
4. You find out in code review—or in production.

**Code Radius fixes the workflow, not the model.** Before edits, the agent must publish a cited **Impact Radius Report** and a **bounded edit set**. After edits, it can verify the diff stayed in-scope.

---

## What you get

| Skill | When it runs | What it does |
| ----- | ------------ | ------------ |
| **`radius`** | Before non-trivial work | Impact Radius Report + edit set / leave-alone list |
| **`radius-gate`** | Mid-task if edits start early | Blocks writes until recon is done (or you override) |
| **`radius-verify`** | After the diff / before PR | Checks scope drift and missing tests from the report |

### Sample output (the viral demo)

Ask:

> Map the Code Radius for adding rate limiting to login. Don’t edit yet.

You get something like:

- **Entry:** `POST /auth/login` → `src/routes/auth.ts:88`
- **Also hits:** OAuth callback + CLI login (shared `issueSession`)
- **Infra:** Kong already limits `/auth/*`
- **Risks:** double limiting on NAT; mobile refresh pattern
- **Edit set:** 4 files · **Leave alone:** gateway + OAuth until product decides
- **Ready to implement?** waiting for go

That screenshot/shareable report is the product.

---

## Install

### Cursor

**Marketplace** (when listed): **Customize** → search **Code Radius** / `code-radius` → **Install** (user or project).

**Local (dev / early access):**

```bash
mkdir -p ~/.cursor/plugins/local
ln -sfn "/absolute/path/to/this/repo" ~/.cursor/plugins/local/code-radius
```

Then **Developer: Reload Window**. Confirm `radius`, `radius-gate`, and `radius-verify` under Customize → Skills.

### Other Agent Plugins clients

Install through that client’s plugin UI, or place this directory where the client loads Agent Plugins (`plugin.json` at the package root).

---

## How to use (daily)

### Automatic

For features, refactors, renames, migrations, auth/payments, or shared modules, the agent should pick up `radius` from the skill description and recon **before** editing.

### Manual prompts that work well

```text
/radius
Map the Code Radius for <change>. Don’t edit yet.
```

```text
Code Radius this: rename UserService → AccountService across the API.
```

```text
Don’t edit—run Code Radius on this ticket first.
```

```text
We think we’re done. Run radius-verify against the edit set.
```

### When to insist on it

- Auth, sessions, permissions, billing
- DB migrations and schema changes
- Renames / moves of shared modules
- Anything that might have a twin in gateway, cron, or mobile
- Onboarding to an unfamiliar area of a large repo (task-scoped, not a full atlas)

### When to skip

- Typo / comment / single-line local fix
- You explicitly say `skip radius` or `just do it`

---

## How it’s different

| Approach | What it optimizes for | Code Radius |
| -------- | --------------------- | ----------- |
| Repo map / onboarding skills | “Explain the architecture” | “What does **this change** touch?” |
| Package / OSS context layers | Dependencies outside your repo | Impact inside **your** working tree |
| Search / grep | Find mentions | Ranked radius + **edit gate** |
| PR bots | Catch damage after the diff | Force recon **before** the diff |

**Unique claim:** task-scoped impact recon as a portable Agent Plugin skill pack—cited report, bounded edit set, optional verify—not another architecture dump.

---

## Package layout

```text
code-radius/
├── plugin.json          # Agent Plugins 1.0.0 manifest
├── LICENSE              # MIT
├── README.md
├── examples/
│   └── demo-prompts.md  # Copy-paste prompts for demos & social
└── skills/
    ├── radius/          # Main impact mapping skill
    │   ├── SKILL.md
    │   └── references/
    ├── radius-gate/     # No-edit-until-mapped
    └── radius-verify/   # Diff vs edit set
```

---

## Publish & share

1. This repo is public: https://github.com/manojnagendra/code-radius-agent-skill
2. Submit to the [Cursor Marketplace](https://cursor.com/marketplace/publish) (open source + review required).
3. Also list on [cursor.directory/plugins/new](https://cursor.directory/plugins/new) if that’s your discovery path.
4. Demo clip: one prompt → Impact Radius Report → “go” → scoped edit → `radius-verify`.
5. One-liner for posts: *Code Radius: don’t let your coding agent edit until it maps the impact radius.*

---

## Spec compliance

- [Agent Plugins 1.0.0](https://agent-plugins.org/) — `plugin.json` + `skills/`
- [Agent Skills](https://agentskills.io/specification) — each `SKILL.md`

## License

MIT — use it, fork it, ship it with your team marketplace.
