# Architecture decisions

Numbered records of adopted decisions. The first ADR usually comes out of the idea debate (`bmad-forge-idea`): adopt / defer / reject the idea and its direction — record it here **before** any further workflow step. Later ADRs follow each adopted architecture spine under `_bmad-output/planning-artifacts/`.

| ID | Title | Status |
|----|-------|--------|
| 001 | Adopt Clean Architecture as the default code structure | adopted |
| 002 | Video-to-Artifact Platform MVP (Voice/Text extraction) | adopted |

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

## ADR-002 — Video-to-Artifact Platform MVP (Voice/Text extraction)

- **Status:** adopted (2026-10-13)
- **Context:** First product idea to emerge from the greenfield workspace. The MVP focuses on a simple, verifiable capability: extract voice/text from video content and produce an artifact (transcript, summary, or structured output). Output/objective remains variable to allow future iterations.
- **Decision:** The MVP will implement a video processing pipeline that guarantees LLM/tool-based video analysis (no "can't see video" fallback). The initial scope is Voice/Text extraction — ingest a video, extract audio/transcript, and produce a text artifact. Architecture will follow Clean Architecture (ADR-001) with inward dependency rule enforced.
- **Binds:** all stories map to the four-ring structure; video processing must use LLM or tool chain for analysis; MVP scope is intentionally narrow (transcript extraction only).
- **Prevents:** expanding MVP scope prematurely (e.g., multi-modal artifacts, real-time streaming, or complex UI); bypassing LLM/tool analysis with hardcoded rules.
- **Note:** This is the first product ADR. Subsequent spine decisions will number from ADR-003 onward.
