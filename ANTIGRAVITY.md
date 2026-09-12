# Google Antigravity / Antigravity CLI

Follow **[`AGENTS.md`](AGENTS.md)** for this workspace.

## Session autoload

At session start, before any task, read `docs/OVERVIEW.md`, `docs/ADR.md`, and `README.md` (see AGENTS.md).

## BMAD bindings

| Surface | Path |
|---------|------|
| Method root | `_bmad/` |
| Google Antigravity skills | `.agent/skills/` |
| Antigravity CLI (AGY) / shared skills | `.agents/skills/` |

## Process

1. Start planned work with `bmad-help`.
2. Cycle: **idea debate → ADR (`docs/ADR.md`) → architecture spine + memlog → spec or epics/stories → implement → evidence**.
3. Before inventing architecture, read `_bmad-output/planning-artifacts/**/ARCHITECTURE-SPINE.md` and `.memlog.md`.
4. Respect context limits: prefer targeted reads over dumping entire trees; keep formal artifacts in `_bmad-output/`.
5. Chat in Spanish when the human writes Spanish; formal BMAD artifacts in English.

## Execution flows

- Planning agents and skills come from installed BMM/CIS/TEA modules — do not fork parallel agent definitions here.
- Implementation stays under `src/` until `AGENTS.md` documents another product root.
