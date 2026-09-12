# BMAD workspace template

Clean-slate repository scaffolded for **BMad Method** multi-agent development. Product code and domain names are intentionally absent — clone or copy this tree, rename the project, then invent architecture through BMAD (debate → spine → spec → implement).

## Layout

| Path | Role |
|------|------|
| `src/` | Product source (stubs only until a product is defined) |
| `docs/` | Long-term project knowledge (`bmm.project_knowledge`) |
| `_bmad/` | BMad Method modules (BMM, CIS, TEA) |
| `_bmad-output/` | Planning, implementation, and test artifacts (English) |
| `.agents/skills/` | Cursor / OpenCode / Pi / Antigravity CLI skills |
| `.claude/skills/` | Claude Code skills |
| `.agent/skills/` | Google Antigravity skills |
| `.opencode/commands/` | OpenCode command bindings |
| `IGNITE.md` | Ignition protocol — mandatory entry point before any planned work |
| `AGENTS.md` | Process rules for coding agents |

## Prerequisites

- Node.js 20.12+
- [uv](https://docs.astral.sh/uv/) (required by BMAD Python skills)
- A supported AI coding tool (Cursor, Claude Code, OpenCode, Antigravity, or Pi)

## Get started

1. Open this folder in your AI tool.
2. Start with **`IGNITE.md`** (referenced as `@IGNITE.md`): the ignition protocol — autoload (`docs/OVERVIEW.md`, `docs/ADR.md`, `README.md`) → `bmad-forge-idea` → ADR + OVERVIEW update. Nothing planned happens before it.
3. Invoke the `bmad-help` skill if unsure what to do next.
4. Prefer: **idea debate → ADR → architecture spine + memlog → spec or epics/stories → implement → evidence**.

Chat may be in Spanish; formal BMAD documents are English.

Authorship is human-only (Sodawave). After clone, enable the trailer-strip hook:

```bash
git config core.hooksPath .githooks
```

## Refresh BMAD

```bash
npx bmad-method install --directory . --yes
```

Use `--modules` / `--tools` when adding modules or IDE bindings. See [BMAD install docs](https://docs.bmad-method.org/start/install-bmad/).
