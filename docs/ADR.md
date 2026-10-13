# Architecture decisions

Numbered records of adopted decisions. The first ADR usually comes out of the idea debate (`bmad-forge-idea`): adopt / defer / reject the idea and its direction — record it here **before** any further workflow step. Later ADRs follow each adopted architecture spine under `_bmad-output/planning-artifacts/`.

| ID | Title | Status |
|----|-------|--------|
| 001 | Adopt Clean Architecture as the default code structure | adopted |

Template spine / memlog path once planning starts:

- `_bmad-output/planning-artifacts/**/ARCHITECTURE-SPINE.md`
- `_bmad-output/planning-artifacts/**/.memlog.md`

## ADR-001 — Adopt Clean Architecture as the default code structure

- **Status:** adopted (2026-10-13)
- **Context:** Greenfield workspace with no product or stack yet. Every future product must share one stable, technology-agnostic code structure so agents and cycles build consistently from day one.
- **Decision:** All product code is implemented per *Clean Architecture: A Craftsman's Guide to Software Structure and Design* (Robert C. Martin; canonical local copy in `docs/`): four rings — Enterprise Core → Application Business Rules → Interface Adapters → Frameworks & Drivers — with the **dependency rule** enforced (dependencies point inward only), a pure core with no framework/DB/UI references, and outer details injected through inner-layer abstractions.
- **Binds:** every story maps its units to a ring; import/dependency checks verify the rule before evidence; the standard is reiterated in `AGENTS.md` ("Code architecture standard").
- **Prevents:** frameworks, ORMs, web servers, or UIs defining business rules or being referenced from the core; per-project one-off layering that contradicts the dependency rule.
- **Note:** workspace-wide coding standard recorded before the product idea gate; the idea debate (IGNITE step 3) takes the next ADR number.
