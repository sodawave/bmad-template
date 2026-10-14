# Architecture Spine: Video-to-Artifact Platform (MVP)

**Created:** 2026-10-13
**Status:** Draft — awaiting human review and approval.
**Branch:** `spec/2026-10-13-video-to-artifact-platform`
**Parent ADR:** [ADR-002](../../docs/ADR.md#adr-002-video-to-artifact-platform-mvp)

---

## 1. Design Principles (from ADR-002 + Clean Architecture)

| Principle | Rationale |
|-----------|-----------|
| **Inward Dependency Rule** (ADR-001) | Core entities and use cases have no dependencies on external frameworks. Adapters implement core interfaces. |
| **Simple MVP** (ADR-002) | Focus on transcript extraction + LLM analysis. No real-time streaming, no multi-tenancy, no cloud storage. |
| **Extensibility** (ADR-002) | New artifact types and transcript services should be addable without changing core logic. |
| **CLI-First** (ADR-002) | No web/Mobile UI for MVP. CLI interface only. |

## 2. Architecture (4-Ring Structure)

```
┌─────────────────────────────────────────────────────────────┐
│  Frameworks / Drivers (Outer Ring)                          │
│  ┌──────────────┐  ┌─────────────────┐                     │
│  │ CLIInterface │  │ ConfigLoader    │                     │
│  └──────┬───────┘  └────────┬────────┘                     │
│         │                   │                               │
├─────────┼───────────────────┼───────────────────────────────┤
│  Adapters (Middle-Outer Ring)                               │
│  ┌───────────────┐  ┌───────────────────┐                 │
│  │VideoFileAdapter│  │TranscriptService  │                  │
│  └───────┬───────┘  │Adapter (LLM)     │                  │
│          │          └────────┬──────────┘                  │
│  ┌───────┴───────┐  ┌────────┴──────────┐                │
│  │ArtifactStorage│  │LLMAnalysisAdapter  │                │
│  │Adapter        │  └────────┬──────────┘                │
│  └───────┬───────┘          │                            │
├─────────┼───────────────────┼─────────────────────────────┤
│  Core / Use Cases (Middle-Inner Ring)                     │
│  ┌───────────────┐  ┌───────────────────┐                │
│  │ProcessVideo   │  │ExtractTranscript   │                │
│  └───────┬───────┘  └────────┬──────────┘                │
│          │                   │                            │
│  ┌───────┴───────┐  ┌────────┴──────────┐                │
│  │AnalyzeWithLLM │  │SaveArtifact        │                │
│  └───────┬───────┘  └────────┬──────────┘                │
├─────────┼───────────────────┼─────────────────────────────┤
│  Core / Entities (Inner Ring)                             │
│  ┌───────────────┐  ┌───────────────────┐                │
│  │Video          │  │Transcript          │                │
│  └───────┬───────┘  └────────┬──────────┘                │
│          │                   │                            │
│  ┌───────┴───────┐  ┌────────┴──────────┐                │
│  │Artifact       │  │User (deferred)    │                │
│  └───────────────┘  └───────────────────┘                │
└─────────────────────────────────────────────────────────────┘
```

## 3. Component Responsibilities

### Core/Entities (Inner Ring)
| Entity | Responsibility |
|--------|----------------|
| `Video` | Represents a video file (path, format, duration). Pure data object. |
| `Transcript` | Represents extracted text from audio (plain text, metadata). Pure data object. |
| `Artifact` | Represents structured analysis result (summary, quotes, timestamps). Pure data object. |
| `User` | (Deferred) Represents a user for multi-tenancy. |

### Core/Use Cases (Middle-Inner Ring)
| Use Case | Responsibility |
|----------|----------------|
| `ProcessVideo` | Orchestrates the full pipeline: upload → extract audio → transcribe → analyze → save. |
| `ExtractTranscript` | Calls transcript service to convert audio to text. |
| `AnalyzeWithLLM` | Sends transcript to LLM for analysis and returns structured artifact. |
| `SaveArtifact` | Saves artifact to local filesystem with metadata. |

### Adapters (Middle-Outer Ring)
| Adapter | Responsibility |
|---------|----------------|
| `VideoFileAdapter` | Reads video file, extracts audio track (FFmpeg wrapper). |
| `TranscriptServiceAdapter` | Calls LLM-based transcription service (OpenAI Whisper, Google Speech). |
| `ArtifactStorageAdapter` | Saves artifact to local filesystem (configurable path). |
| `LLMAnalysisAdapter` | Calls LLM for analysis with prompt template. |

### Frameworks/Drivers (Outer Ring)
| Component | Responsibility |
|-----------|----------------|
| `CLIInterface` | Handles CLI commands, arguments, and output. |
| `ConfigLoader` | Loads configuration (LLM provider, storage path). |

## 4. Dependency Flow (Inward Rule)

```
CLIInterface → ProcessVideo → VideoFileAdapter
ConfigLoader → ExtractTranscript → TranscriptServiceAdapter
ProcessVideo → AnalyzeWithLLM → LLMAnalysisAdapter
ProcessVideo → SaveArtifact → ArtifactStorageAdapter

Core/Entities (Video, Transcript, Artifact) ← no dependencies on adapters or frameworks.
```

## 5. Technology Stack (MVP)

| Layer | Technology | Rationale |
|-------|------------|-----------|
| **Language** | Node.js (TypeScript) | Consistency with BMAD ecosystem. |
| **CLI Framework** | Commander.js | Simple, well-documented CLI interface. |
| **Video Processing** | FFmpeg (via `fluent-ffmpeg`) | Industry-standard video/audio extraction. |
| **Transcription** | OpenAI Whisper API (or local model) | High-quality transcription. |
| **LLM Analysis** | OpenAI GPT-4 / Claude API (configurable) | Structured text analysis. |
| **Storage** | Local filesystem (`fs` module) | Simple, no database dependency. |
| **Testing** | Jest + Supertest (CLI) | Unit and integration tests. |

## 6. File Structure (Proposed)

```
src/
├── core/
│   ├── entities/
│   │   ├── Video.ts
│   │   ├── Transcript.ts
│   │   ├── Artifact.ts
│   │   └── User.ts (deferred)
│   └── usecases/
│       ├── ProcessVideo.ts
│       ├── ExtractTranscript.ts
│       ├── AnalyzeWithLLM.ts
│       └── SaveArtifact.ts
├── adapters/
│   ├── VideoFileAdapter.ts
│   ├── TranscriptServiceAdapter.ts
│   ├── ArtifactStorageAdapter.ts
│   └── LLMAnalysisAdapter.ts
├── frameworks/
│   ├── CLIInterface.ts
│   └── ConfigLoader.ts
├── index.ts (entry point)
└── config/
    └── default.json (LLM provider, storage path)
```

## 7. Open Questions (for Human Review)

1. **LLM Provider:** Which LLM service for transcription? (Suggested: OpenAI Whisper API.)
2. **LLM Analysis:** Which LLM service for analysis? (Suggested: OpenAI GPT-4 / Claude API.)
3. **CLI Framework:** Node.js (Commander) or Python (Click)? (Suggested: Node.js.)
4. **Video Formats:** Which formats to support? (Suggested: MP4, AVI, MKV.)
5. **Artifact Format:** JSON or Markdown? (Suggested: JSON for parsing, Markdown for readability.)

---

**Next Step:** Human reviews this spine, approves/rejects/modifies, then proceeds to implementation with evidence (tests).
