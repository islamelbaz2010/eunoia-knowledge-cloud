# System Overview — Eunoia Knowledge Cloud

## Purpose

Eunoia Knowledge Cloud (EKC) is the Global Knowledge Production Platform responsible for producing, validating, versioning, and publishing reusable Knowledge Packs consumed by Eunoia AI OS and downstream systems. EKC centralizes knowledge authoring, governance, and packaging for internal and commercial distribution while preserving strict separation from runtime AI services, connectors, and data stores.

## Vision

Provide a production-grade, automated platform that transforms curated source materials into auditable, versioned, validated Knowledge Packs. EKC will scale to serve enterprise customers and a marketplace while maintaining rigorous quality controls, provenance, and extensible taxonomy for global coverage.

## Primary Business Goals

- Maximize scalability, maintainability, and automation.
- Enable commercial Knowledge-as-a-Service and marketplace monetization.
- Provide a single source of truth for reusable knowledge artifacts consumed by Eunoia AI OS.

## Major Components

- Generator: deterministic pipeline that produces Knowledge Packs from registered sources and datasets.
- Registry: authoritative indices for Sources, Datasets, Packs, Templates, Schemas, Versions, and Taxonomy.
- Validator: multi-stage quality and policy verification engine with configurable quality levels.
- Publisher: artifacts distribution, signing, and integrity assurance service.
- Filesystem: canonical storage layout for artifacts and temporary workspace staging.
- Orchestration & Pipelines: event-driven runners that manage staged production and retries.
- Governance & Configuration: policy, roles, taxonomy, and release controls.

## System Boundaries

In KC-0 the system boundaries are intentionally narrow: EKC is architecture-only. EKC must not contain connectors, crawlers, AI, embeddings, vector search, databases, Supabase, SaaS APIs, or production runtime services. EKC defines interfaces, schemas, and events to which those future systems will conform.

External systems (out of scope for KC-0):

- Source connectors and crawlers that provide raw content.
- AI services for generation, embeddings, and inference.
- Vector stores and search indexes.
- External database or SaaS hosting services.

## Data Flow (conceptual)

1. Source Registration: Author registers a Source in the Source Registry with metadata and manifest pointers.
2. Dataset Preparation: Datasets are registered/derived and referenced by the Generator.
3. Generation Pipeline: Generator reads Source/Template/Schema and emits candidate Pack artifacts into staging.
4. Validation: Validator consumes staged artifacts, runs checks, and returns rich validation reports.
5. Pack Build: Valid artifacts are packaged into immutable Knowledge Packs and checksummed.
6. Publish: Publisher signs and distributes packs to the Pack Registry and distribution targets.
7. Consumption: Eunoia AI OS or other clients download packs via the Pack Registry API and consume per the contract.

## Non-Functional Requirements (high-level)

- Scalability: pipelines scale horizontally; registries scale to millions of artifacts.
- Determinism: generation is deterministic given same inputs and configuration.
- Auditability: all operations produce immutable provenance metadata and logs.
- Security: artifact integrity, signing, and role-based access controls.
- Extensibility: taxonomy, templates, and validation rules are pluggable.

---
---
## Enterprise & Security Additions

- **Enterprise features required:** RBAC, SSO integration, audit logging, tenant isolation modes (single-tenant vs multi-tenant), rate limits, quotas, billing metadata, and SLA definitions must be provided by implementation teams.
- **Key management & signing discovery:** The architecture mandates a signer key management strategy and a registry-backed public key discovery mechanism for consumers; the Publisher and Pack Registry will publish key metadata for verification.
- **Compliance & data residency:** Packs and registries must support region tagging, retention policies, and export controls to enable compliance (GDPR, data residency laws).

## Observability & SLOs

- **Telemetry:** All components emit traces, metrics, and structured logs with `correlationId` and `traceId` included in manifests and events.
- **SLOs & SLA readiness:** Implementations must define SLOs for generation latency, registry availability, and pack retrieval; operational runbooks and incident handling are required.

_This overview establishes the scope and principal components. Deeper behavior is defined in the companion documents in this directory._
