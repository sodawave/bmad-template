# Spec: Video-to-Artifact Platform (MVP)

**Created:** 2026-10-13
**Status:** Draft — awaiting human review and approval.
**Branch:** `spec/2026-10-13-video-to-artifact-platform`
**Parent ADR:** [ADR-002](../../docs/ADR.md#adr-002-video-to-artifact-platform-mvp)

---

## 1. Problem Statement

Users upload a video and want to extract structured, reusable artifacts (text summaries, key quotes, timestamps) without manually watching the entire video. The MVP focuses on **transcript extraction + LLM-based analysis** to produce a text artifact from video input.

## 2. Scope (MVP)

### In Scope
- Upload a video file (local path or URL).
- Extract audio track from video.
- Transcribe audio to text (via LLM/tool chain).
- Analyze transcript with LLM to produce a structured text artifact (summary, key quotes, timestamps).
- Save artifact locally (filesystem storage).
- CLI interface for users.

### Out of Scope (Deferred)
- Real-time streaming processing.
- Multi-modal artifacts (images, audio-only).
- User authentication / multi-tenancy.
- Web/Mobile UI (CLI only for MVP).
- Cloud storage (S3, databases) — local filesystem only.

## 3. Functional Requirements

### FR-01: Video Upload
- **As a** user,
- **I want to** upload a video file (local path or URL),
- **So that** the system can process it for artifact extraction.

**Acceptance Criteria:**
- [ ] CLI accepts video file path as argument.
- [ ] CLI accepts video URL as argument (optional, deferred if complex).
- [ ] System validates file format (MP4, AVI, MKV, etc.).
- [ ] Error message if file is invalid or unreadable.

### FR-02: Audio Extraction
- **As a** system,
- **I want to** extract the audio track from the video file,
- **So that** I can transcribe it to text.

**Acceptance Criteria:**
- [ ] System extracts audio from supported video formats.
- [ ] Audio is saved temporarily for transcription.
- [ ] Temporary files are cleaned up after processing.

### FR-03: Transcript Extraction
- **As a** system,
- **I want to** transcribe the audio track to text using an LLM/tool chain,
- **So that** I can analyze the content and produce artifacts.

**Acceptance Criteria:**
- [ ] System calls transcript service (LLM-based, not hardcoded rules).
- [ ] Transcript is returned as plain text.
- [ ] Error handling if transcription fails (retry or fallback).

### FR-04: LLM Analysis
- **As a** system,
- **I want to** analyze the transcript with an LLM to produce structured artifacts,
- **So that** users get useful, reusable content from the video.

**Acceptance Criteria:**
- [ ] System sends transcript to LLM with analysis prompt.
- [ ] LLM returns structured artifact (summary, key quotes, timestamps).
- [ ] Artifact format is consistent and parseable (JSON/markdown).

### FR-05: Artifact Storage
- **As a** user,
- **I want to** save the generated artifact locally,
- **So that** I can access it later for reuse.

**Acceptance Criteria:**
- [ ] System saves artifact to local filesystem (configurable path).
- [ ] Artifact is saved with metadata (video title, timestamp, source).
- [ ] System returns path to saved artifact.

### FR-06: CLI Interface
- **As a** user,
- **I want to** interact with the system via command line,
- **So that** I can process videos without a web UI.

**Acceptance Criteria:**
- [ ] CLI command: `video-to-artifact process <video-path>`.
- [ ] CLI outputs progress and results to stdout.
- [ ] CLI returns exit code 0 on success, non-zero on error.

## 4. Non-Functional Requirements

### NFR-01: Performance
- MVP should process a 5-minute video in under 2 minutes (transcription + analysis).
- No hard real-time constraint; batch processing is acceptable.

### NFR-02: Reliability
- System should handle errors gracefully (invalid video, transcription failure).
- Retry mechanism for transient LLM API failures (max 2 retries).

### NFR-03: Extensibility
- New artifact types (e.g., audio-only, image extraction) should be addable without changing core logic.
- New transcript services (e.g., Whisper, Google Speech) should be swappable via adapter pattern.

### NFR-04: Clean Architecture Compliance
- All code must follow inward dependency rule (ADR-001).
- Core entities and use cases are framework-independent.
- Adapters implement interfaces defined in core.

## 5. Architecture Mapping (4-Ring Structure)

| Ring | Components | Responsibility |
|------|------------|----------------|
| **Core/Entities** | `Video`, `Transcript`, `Artifact`, `User` (deferred) | Business objects, rules. |
| **Core/Use Cases** | `ProcessVideo`, `ExtractTranscript`, `AnalyzeWithLLM`, `SaveArtifact` | Orchestrate entities, business logic. |
| **Adapters** | `VideoFileAdapter`, `TranscriptServiceAdapter` (LLM), `ArtifactStorageAdapter` (filesystem) | Translate between core and external systems. |
| **Frameworks/Drivers** | `CLIInterface`, `ConfigLoader` | User interface, infrastructure setup. |

## 6. Epics & Stories (Draft)

### Epic 1: Video Upload & Processing Pipeline
- **Story 1.1:** As a user, I can upload a video file via CLI and see progress.
- **Story 1.2:** As a system, I can extract audio from supported video formats.

### Epic 2: Transcript Extraction
- **Story 2.1:** As a system, I can transcribe audio to text using an LLM service.
- **Story 2.2:** As a system, I can handle transcription errors and retry.

### Epic 3: LLM Analysis & Artifact Generation
- **Story 3.1:** As a system, I can analyze transcript with LLM and produce structured artifact.
- **Story 3.2:** As a user, I can view the generated artifact (summary, quotes, timestamps).

### Epic 4: Artifact Storage & CLI Interface
- **Story 4.1:** As a user, I can save the artifact to local filesystem with metadata.
- **Story 4.2:** As a user, I can interact with the system via CLI commands.

## 7. Verification Plan

| Story | Test Type | Evidence |
|-------|-----------|----------|
| 1.1, 1.2 | Integration test (video upload + audio extraction) | CLI processes valid video, returns success. |
| 2.1, 2.2 | Integration test (transcription) | Transcript matches audio content (manual review). |
| 3.1, 3.2 | Integration test (LLM analysis) | Artifact contains summary, quotes, timestamps. |
| 4.1, 4.2 | Integration test (storage + CLI) | Artifact saved to disk, CLI returns path. |

## 8. Open Questions (for Human Review)

1. **Video formats:** Which video formats to support in MVP? (Suggested: MP4, AVI, MKV.)
2. **LLM service:** Which LLM provider for transcription? (Suggested: OpenAI Whisper, Google Speech-to-Text.)
3. **Artifact format:** JSON or Markdown for structured artifact? (Suggested: JSON for parsing, Markdown for readability.)
4. **CLI framework:** Node.js (Commander) or Python (Click)? (Suggested: Node.js for consistency with BMAD ecosystem.)
5. **Temporary files:** Where to store temporary audio/transcript files? (Suggested: OS temp directory, cleaned up after processing.)

---

**Next Step:** Human reviews this spec, approves/rejects/modifies, then proceeds to implementation with evidence (tests).
