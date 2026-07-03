# Layered Architecture

This document describes the logical layers that compose EKC, their responsibilities, and interactions. The layer model enforces separation of concerns, enabling independent scaling, clear ownership, and testability.

## Layers Overview

1. Presentation
2. CLI & Developer Tools
3. Orchestration
4. Pipeline (Generator & Workers)
5. Registry
6. Validation
7. Generation (pack builder)
8. Publishing
9. Filesystem & Artifact Storage
10. Infrastructure & Deployment

### 1. Presentation

- Responsibilities: human-facing documentation and administrative UI prototypes for governance, registry dashboards, and validation reports.
- Constraints: read-only in KC-0; no SaaS features implemented.

### 2. CLI & Developer Tools

- Responsibilities: local authorship workflows, pack staging, manifest inspection, and release triggers.
- Interfaces: CLI commands for `register-source`, `validate-pack`, `build-pack`, `publish-pack`.

### 3. Orchestration

- Responsibilities: coordinate event-driven flows, schedule retries, coordinate long-running jobs, and maintain pipeline state.
- Implementation notes: thin orchestration layer emitting and responding to domain events; must support idempotency and idempotent consumers.

### 4. Pipeline (Generator & Workers)

- Responsibilities: execute deterministic generation tasks, transform datasets through templating stages, and emit staged artifacts.
- Characteristics: horizontally scalable, stateless workers, stable artifact semantics.

### 5. Registry

- Responsibilities: authoritative indices of sources, datasets, packs, templates, schemas, versions, and taxonomy.
- Interfaces: read-optimized APIs (internal) for discovery and resolution.

### 6. Validation

- Responsibilities: policy and quality checks, multi-tiered scoring, and producing structured validation reports with actionable fixes.

### 7. Generation (Pack Builder)

- Responsibilities: assemble validated artifacts into immutable Knowledge Packs, compute checksums, sign metadata, and record provenance.

### 8. Publishing

- Responsibilities: manage canonical pack distribution, signing, integrity verification, and version promotion.

### 9. Filesystem & Artifact Storage

- Responsibilities: canonical directory layout, workspace staging areas, and immutable pack blobs.
- Characteristics: simple POSIX-like layout for KC-0; storage abstraction in future phases.

### 10. Infrastructure & Deployment

- Responsibilities: CI/CD pipelines for EKC, artifact storage, secrets management, and observability (logs, metrics, traces).

## Dependencies and Responsibilities

- Layers depend only on lower layers by API. No downward calls.
- All side-effects, external calls, and stateful persistence are performed by the infrastructure layer or dedicated services (out of scope in KC-0).

## Security and Governance

- Role-based access controls applied at presentation and registry boundaries.
- Signing and integrity checks applied at pack build and publish stages.

---
### Observability & SLO Layer

- Responsibilities: centralized telemetry collection, alerting, tracing, and compliance reporting. Define SLOs for core flows and provide dashboards and runbooks.

### Enterprise Operations

- Responsibilities: role and policy administration (RBAC), key management adapters, tenant lifecycle operations, and backup/restore orchestration.

_Layer definitions provide the foundation for module-level specifications in other documents._
