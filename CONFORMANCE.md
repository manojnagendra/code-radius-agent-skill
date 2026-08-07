# Conformance — Agent Plugins + Agent Skills

This package targets the vendor-neutral standards, not a single IDE.

## Agent Plugins 1.0.0

Checklist against [agent-plugins.org](https://agent-plugins.org/):

- [x] `plugin.json` at package root
- [x] `$schema` is exactly `https://agent-plugins.org/schemas/1.0.0/plugin.schema.json`
- [x] `name` is `code-radius` (lowercase kebab-case, valid pattern)
- [x] Skills live under `skills/<skill-name>/SKILL.md` (immediate children only)
- [x] No requirement on `.cursor-plugin/` (that is Cursor’s parallel format)
- [x] No client `extensions` block required for core behavior
- [x] Optional `mcp.json` omitted (skills-only plugin is valid)

## Agent Skills

Checklist against [agentskills.io/specification](https://agentskills.io/specification):

- [x] Each skill directory name matches frontmatter `name`
- [x] Required `name` + `description` in YAML frontmatter
- [x] Optional `license` / `metadata` used consistently
- [x] Progressive disclosure: detailed playbooks in `references/` for `radius`

## Portable vs Cursor-only

| Format | Manifest | This repo |
| ------ | -------- | --------- |
| **Agent Plugin** (open standard) | Root `plugin.json` | **Yes — this is what we ship** |
| Cursor Plugin (Cursor-specific) | `.cursor-plugin/plugin.json` | No (not needed for skills portability) |

Clients that support Agent Plugins load this package as-is. Cursor supports both formats; Agent Plugins load in Cursor without a Cursor-only wrapper.
