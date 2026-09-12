# Project overview

Session-autoload file — every agent reads this (plus `docs/ADR.md` and `README.md`) before taking any task. See "Session autoload" in `AGENTS.md`.

- **Product:** undefined — greenfield BMAD workspace template. A product is adopted via BMAD planning; implementation then lands under `src/`.
- **Audience / rigor:** launch-grade. Verify assumptions against the repo; prefer evidence over claims.
- **Status:** clean slate — no stack, no architecture decisions, no runtime code (`src/` holds stubs only).
- **Language:** chat may be Spanish; this file and all BMAD artifacts are English.

## Where truth lives

| Question | Source |
|---|---|
| What is this project? | This file, `README.md` |
| What has been decided, and why? | `docs/ADR.md`, `_bmad-output/planning-artifacts/**/ARCHITECTURE-SPINE.md` + `.memlog.md` |
| How must agents behave? | `AGENTS.md` (and its tool wrappers) |
| What code exists? | `src/` |
| What is being built right now? | `_bmad-output/` cycle artifacts |

Keep this file current whenever product identity, audience, or status changes.
