# Code Radius

**Don’t edit until you know what the change will touch.**

Code Radius is an **[Agent Plugin](https://agent-plugins.org/)** (open standard v1.0.0). It packages portable **[Agent Skills](https://agentskills.io/)** so any compatible agent client can load the same plugin—without a Cursor-only manifest, MCP server, or API keys.

Compatible clients at the Agent Plugins launch include ChatGPT/Codex, Cursor, GitHub Copilot, Kiro, and VS Code. One package; each client discovers `plugin.json` and `skills/` the same way.

---

## Open standard conformance

This repo is intentionally **not** a Cursor-only plugin (no `.cursor-plugin/` required).

| Spec | What we ship |
| ---- | ------------ |
| [Agent Plugins 1.0.0](https://agent-plugins.org/) | Root [`plugin.json`](./plugin.json) with `$schema`: `https://agent-plugins.org/schemas/1.0.0/plugin.schema.json` |
| [Agent Skills](https://agentskills.io/specification) | [`skills/*/SKILL.md`](./skills/) (+ optional `references/`) |
| Optional MCP | None (not required for this plugin) |
| Client extensions | None (no reverse-domain Cursor-only extras in the portable contract) |

Layout:

```text
code-radius/
├── plugin.json          # Agent Plugins manifest (required)
├── skills/
│   ├── radius/          # Agent Skill
│   ├── radius-gate/
│   └── radius-verify/
├── examples/
├── LICENSE              # MIT
└── README.md
```

---

## What it does

AI agents are good at *finding* files and bad at *scoping* a change. They often edit after a shallow search, miss callers and tests, and you discover the real impact in review.

**Code Radius** makes **task-scoped impact recon** a first-class step:

1. **`radius`** — cited Impact Radius Report + bounded edit set **before** edits  
2. **`radius-gate`** — stop mid-task if edits start without a report or drift past the edit set  
3. **`radius-verify`** — after the diff, confirm it stayed in-scope  

### Example

> Map the Code Radius for adding rate limiting to login. Don’t edit yet.

You get entry points, sibling surfaces, infra twins, test gaps, risks, an **edit set**, and **leave alone**—then the agent waits for go.

---

## Install (any Agent Plugins client)

Install via that client’s plugin UI using this repository, or place this directory where the client loads Agent Plugins (manifest at package root).

### Codex / ChatGPT Work

You are adding a **plugin marketplace**, not pasting a random sparse path. This repo includes Codex’s catalog at [`.agents/plugins/marketplace.json`](./.agents/plugins/marketplace.json).

In **Add plugin marketplace**:

| Field | Value |
| ----- | ----- |
| **Source** | `https://github.com/manojnagendra/code-radius-agent-skill` |
| **Git ref** | `main` |
| **Sparse paths** | **leave empty** (do not use `plugins/codex` — that path does not exist) |

Then open the **Code Radius** marketplace source and install the `code-radius` plugin. Start a **new** chat and ask: *Map the Code Radius for … Don’t edit yet.*

CLI equivalent:

```bash
codex plugin marketplace add https://github.com/manojnagendra/code-radius-agent-skill --ref main
```

### Cursor (one supported client)

**Marketplace / Directory:** search **Code Radius** / `code-radius`, or use [cursor.directory/plugins/code-radius](https://cursor.directory/plugins/code-radius).

**Local load:** Cursor only loads plugins **inside** `~/.cursor/plugins/local/` (symlinks pointing outside that folder are rejected):

```bash
mkdir -p ~/.cursor/plugins/local/code-radius
rsync -a --delete --exclude '.git' --exclude 'logo.png' ./ ~/.cursor/plugins/local/code-radius/
```

Then reload the window. Skills: `radius`, `radius-gate`, `radius-verify`.

---

## How to use

**Automatic:** For features, refactors, renames, migrations, auth/payments, or shared modules, the agent should pick up `radius` from the skill description and recon before editing.

**Manual:**

```text
Map the Code Radius for <change>. Don’t edit yet.
```

```text
Run radius-verify against the last edit set.
```

More prompts: [`examples/demo-prompts.md`](./examples/demo-prompts.md).

---

## How it’s different

| Approach | Optimizes for | Code Radius |
| -------- | ------------- | ----------- |
| Repo map / onboarding skills | “Explain the architecture” | “What does **this change** touch?” |
| OSS/package context layers | Dependencies outside your repo | Impact inside **your** tree |
| Search / grep | Find mentions | Ranked radius + **edit gate** |
| PR bots | Catch damage after the diff | Force recon **before** the diff |

---

## License

MIT — portable under the Agent Plugins / Agent Skills ecosystem.
