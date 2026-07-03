# Event-Driven Architecture

EKC is designed as an event-driven production pipeline where domain events coordinate loosely coupled components. Events are the primary integration mechanism between modules; they are durable, idempotent, and richly typed.

## Event Principles

- Events are immutable and contain minimal, resolvable references (IDs and version tags).
- Consumers must be idempotent; all events include a monotonic `eventId` and `timestamp`.
- Events are published to an internal event bus; in KC-0 they are specified and described (no implementation).

## Core Events

- `SourceRegistered` — emitted when a new Source is added to the Source Registry.
- `DatasetPrepared` — emitted when a dataset is resolved and made available for generation.
- `GenerationRequested` — request to generate artifacts for a source/dataset.
- `DatasetGenerated` — generator completed and staged artifacts available.
- `ValidationPassed` — validator passed; includes `validationReportRef` and score.
- `ValidationFailed` — validator failed; includes failure reasons and remediation hints.
- `PackBuilt` — builder created an immutable pack with manifest and checksums.
- `PackPublished` — publisher successfully published a pack to a channel.
- `PackDeprecated` — a published pack was deprecated or revoked.

## Event Payload Design (common fields)

- `eventId` (UUID): unique and stable per event emission.
- `source` (string): originating service or actor id.
- `targetRef` (object): canonical reference (type, id, version).
- `provenance` (object): pointers to inputs (manifest, datasetRef, templateRef).
- `metadata` (object): arbitrary indexed metadata for routing and observability.

## Producer / Consumer Relationships

- Source Registry produces `SourceRegistered`. Consumers: Generator, Taxonomy.
- Dataset service produces `DatasetPrepared`. Consumers: Generator, Validator.
- Generator produces `DatasetGenerated`, `GenerationFailed`. Consumers: Validator, Registry.
- Validator produces `ValidationPassed`/`ValidationFailed`. Consumers: Pack Builder, Publisher.
- Pack Builder produces `PackBuilt`. Consumers: Publisher, Pack Registry.
- Publisher produces `PackPublished`/`PackDeprecated`. Consumers: Pack Registry, downstream monitors.

## Error Handling & Retries

- Events include retryable and terminal error categories.
- Orchestration layer implements exponential backoff with bounded retries; persistent failures are escalated via `ValidationFailed` or `PackDeprecated` events.

## Observability

- Every event must correlate to a `correlationId` for tracing across steps.
- Events must carry execution metrics where appropriate (timings, size, quality score).

---
_Events are the canonical integration fabric for EKC; the event catalog above forms the contract between components._
