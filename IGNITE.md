# Ignite — how this workspace starts

Ignition point of the BMAD workflow. Nothing planned happens before this runs.

## Ignition protocol

1. **Autoload first:** read `docs/OVERVIEW.md`, `docs/ADR.md`, `README.md` (see "Session autoload" in `AGENTS.md`).
2. **Forge the idea:** run `bmad-forge-idea` to pressure-test it. Inputs: the identity/status in `docs/OVERVIEW.md` and any prior ADRs in `docs/ADR.md`.
3. **Record the outcome:** the debate result becomes a numbered ADR in `docs/ADR.md` (adopt / defer / reject + direction), and `docs/OVERVIEW.md` is updated to reflect the new identity and status.
4. **Then, and only then:** continue the cycle — architecture spine + memlog → spec or epics/stories → implement → evidence.

## Rules

- No planning artifact (brief, PRD, spine, spec, epic) may be written before step 3 records the decision.
- If the forge debate would change neither `docs/OVERVIEW.md` nor `docs/ADR.md`, the idea was not really tested — run it again or drop it.
- This file is the single ignition entry point; tool wrappers point here for "where do I start".
- **Bootstrap-only:** once the first ADR is recorded and `docs/OVERVIEW.md` updated, this file may be deleted or ignored. Do not add it to permanent autoload/imports.
