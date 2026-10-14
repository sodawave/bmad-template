# Memlog: Video-to-Artifact Platform (MVP)

**Created:** 2026-10-13
**Branch:** `spec/2026-10-13-video-to-artifact-platform`
**Parent ADR:** [ADR-002](../../docs/ADR.md#adr-002-video-to-artifact-platform-mvp)

---

## 1. Decision Log (Chronological)

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-10-13 | **Idea:** Video-to-Artifact Platform (MVP: Voice/Text extraction) | Simple MVP, variable output. |
| 2026-10-13 | **ADR-002:** Approved MVP scope (transcript + LLM analysis) | Follows BMAD idea gate. |
| 2026-10-13 | **Architecture:** 4-ring Clean Architecture (entities → use cases → adapters → frameworks) | ADR-001 mandatory. |
| 2026-10-13 | **Technology:** Node.js (TypeScript), FFmpeg, OpenAI Whisper API, GPT-4/Claude API | Consistency with BMAD ecosystem. |
| 2026-10-13 | **File Structure:** `src/core/`, `src/adapters/`, `src/frameworks/` | Clean Architecture standard. |

## 2. Open Questions (for Human Review)

1. **LLM Provider:** Which LLM service for transcription? (Suggested: OpenAI Whisper API.)
2. **LLM Analysis:** Which LLM service for analysis? (Suggested: OpenAI GPT-4 / Claude API.)
3. **CLI Framework:** Node.js (Commander) or Python (Click)? (Suggested: Node.js.)
4. **Video Formats:** Which formats to support? (Suggested: MP4, AVI, MKV.)
5. **Artifact Format:** JSON or Markdown? (Suggested: JSON for parsing, Markdown for readability.)

## 3. Risks & Mitigations

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|-----------|
| LLM API rate limits | High | Medium | Add retry logic, queue system. |
| Large video files (slow processing) | High | Medium | Add progress indicator, chunked processing. |
| FFmpeg not installed on user's system | Medium | Low | Add installation check, error message. |
| LLM API cost (per video) | Medium | High | Add cost estimate, user confirmation. |

## 4. Future Considerations (Out of Scope for MVP)

- Real-time video streaming
- Multi-user / multi-tenancy
- Cloud storage (S3, GCS)
- Web/Mobile UI
- Video editing / annotation
- Social sharing

---

**Next Step:** Human reviews this memlog, approves/rejects/modifies, then proceeds to implementation with evidence (tests).
