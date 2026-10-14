# Architecture Spine — Video-to-Artifact Platform (MVP)

- **Status:** adopted (2026-10-13)
- **Parent ADRs:** ADR-001 (Clean Architecture), ADR-002 (MVP scope: Voice/Text extraction)
- **Scope:** MVP — Video → Transcript artifact (LLM/tool-based analysis guaranteed)

## Four-Ring Structure (Clean Architecture)

### 1. Enterprise Core (Entities)
**Responsabilidad:** Reglas de negocio más puras, independientes de frameworks, DB o UI.

| Entity | Descripción |
|--------|-------------|
| `VideoArtifact` | Representación interna del artifact de texto generado (transcript, summary, structured output). Contiene metadata (id, timestamp, source_video_id) y contenido (text, format_type). |
| `ProcessingPipeline` | Define las etapas del pipeline: ingest → extract → analyze → produce. Cada etapa tiene un estado (pending, running, completed, failed) y logs de ejecución. |
| `ArtifactSpec` | Especificación del tipo de artifact a generar (transcript, summary, key_points). Define reglas de validación y formato de salida. |

### 2. Application Business Rules (Use Cases)
**Responsabilidad:** Lógica específica del MVP — orquesta los casos de uso que cumplen los objetivos de negocio.

| Use Case | Descripción |
|----------|-------------|
| `ProcessVideo` | Caso de uso principal: ingesta un video, ejecuta el pipeline (extract audio → transcribe → analyze with LLM), y produce un artifact de texto. |
| `GenerateTranscript` | Extrae audio del video y lo transcribe usando un servicio de speech-to-text. Valida la calidad del transcript (longitud mínima, idioma detectado). |
| `AnalyzeWithLLM` | Envía el transcript a un LLM para generar insights, resúmenes o estructura según `ArtifactSpec`. |
| `ValidateArtifact` | Verifica que el artifact cumpla con las reglas de negocio (formato, contenido mínimo, metadata completa). |

### 3. Interface Adapters (Adapters)
**Responsabilidad:** Convierte datos entre los casos de uso y los frameworks/UI. Implementa interfaces definidas en el core.

| Adapter | Implementación | Interface que implementa |
|---------|----------------|--------------------------|
| `VideoIngestAdapter` | Lee archivos de video (MP4, MOV, AVI) y los convierte a formato interno para procesamiento. | `IVideoSource` (definida en core) |
| `AudioExtractAdapter` | Extrae pista de audio del video y la convierte a formato procesable (WAV, FLAC). | `IAudioExtractor` |
| `TranscriptServiceAdapter` | Wrapper para servicio de speech-to-text (OpenAI Whisper, Google STT, etc.). | `ITranscriptService` |
| `LLMClientAdapter` | Wrapper para cliente LLM (OpenAI, Anthropic, local models). Envía prompts y recibe respuestas estructuradas. | `ILLMClient` |
| `ArtifactStorageAdapter` | Persiste artifacts en storage (filesystem, S3, database). Serializa/deserializa `VideoArtifact`. | `IArtifactRepository` |

### 4. Frameworks & Drivers (Frameworks)
**Responsabilidad:** Detalles externos — frameworks, librerías, servicios de terceros.

| Componente | Tecnología | Propósito |
|------------|------------|-----------|
| Video Processing | FFmpeg (lib) | Ingesta y extracción de audio |
| Speech-to-Text | OpenAI Whisper API / local model | Transcripción de audio a texto |
| LLM Client | OpenAI API / Anthropic API / Ollama (local) | Análisis de transcript → artifact |
| Storage | Local filesystem (MVP) / S3 (future) | Persistencia de artifacts |
| CLI Interface | Node.js/Python CLI (MVP) | Interfaz de línea de comandos para usuarios |

## Dependency Rule Enforcement

- **Core → Use Cases:** Las entidades (`VideoArtifact`, `ProcessingPipeline`) son puras — no importan interfaces de adapters.
- **Use Cases → Adapters:** Los casos de uso definen interfaces (`IVideoSource`, `ITranscriptService`) que los adapters implementan.
- **Adapters → Frameworks:** Los adapters importan librerías externas (FFmpeg, Whisper API, LLM SDK).
- **Inward only:** Las flechas de dependencia apuntan hacia el centro (core). Los adapters son "inyectados" en los use cases.

## MVP Scope Boundaries (ADR-002)

**In scope (MVP):**
- Ingestar un video local (archivo).
- Extraer audio y transcribirlo.
- Generar un artifact de texto (transcript crudo o resumen básico).
- CLI interface para usuarios.

**Out of scope (deferred):**
- Streaming en tiempo real.
- Múltiples modalidades de artifact (video, audio, imágenes).
- UI web o móvil.
- Autenticación/autorización de usuarios.
- Multi-tenancy o storage distribuido.

## Next Steps (Spec/Epics)

1. Desglosar en historias de usuario verificables (test-driven).
2. Definir interfaces de cada adapter en el core.
3. Implementar MVP con storage local y CLI interface.
