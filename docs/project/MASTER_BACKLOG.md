# Master Backlog — Eunoia Knowledge Cloud

This backlog captures all planned work across KC-0 through KC-4 phases, organized by component and phase. Work items are tracked by phase and link to sprint boards and implementation tasks.

## KC-0 — Architecture Sprint (COMPLETE)

All architecture and specification tasks are complete. See `docs/architecture/`.

- ✅ System overview, business goals, and data flows documented.
- ✅ Layered architecture defined.
- ✅ Component specifications with responsibilities and interfaces.
- ✅ Event-driven integration fabric designed.
- ✅ Registry architecture and multi-registry model.
- ✅ Generator pipeline specification.
- ✅ Knowledge Pack format and manifest specification.
- ✅ Taxonomy architecture.
- ✅ Validation specification (quality gates and tiers).
- ✅ Publisher specification (signing, channels, distribution).
- ✅ Eunoia AI OS consumption contract.
- ✅ Roadmap and phase definitions.
- ✅ Architecture Review completed and documented.
- ✅ Architecture frozen and versioned.

---

## KC-1 — Reference Implementation

### Registry & Persistence

- Implement file-backed Source Registry with versioning and metadata validation.
- Implement file-backed Dataset Registry.
- Implement file-backed Pack Registry.
- Implement file-backed Template Registry.
- Implement file-backed Schema Registry.
- Implement file-backed Taxonomy Registry.
- Implement Version Registry (time-series snapshots).
- Define registry migration adapter interface for future datastores.

### Generator Pipeline

- Implement Generator orchestration (input resolution, pre-flight validation).
- Implement Template rendering engine with determinism guarantees.
- Implement deterministic artifact generation and staging.
- Implement configuration loader and versioning.
- Implement generation reporting and logging.

### Validator

- Implement schema validation (JSON Schema against registered schemas).
- Implement policy checks (licensing, content type allowlists).
- Implement quality heuristics (content length, link checks, alt-text).
- Implement deduplication (checksum matching and similarity heuristics).
- Implement ValidationReport generation with severity levels.

### Publisher & Pack Builder

- Implement Pack Builder (assemble artifacts, compute checksums).
- Implement Manifest generation and validation.
- Implement Publisher (channel promotion: draft → staged → stable).
- Implement stub signing (local algorithm-agnostic interface).
- Implement PublishReceipt generation.

### CLI & Orchestration

- Implement CLI commands: `register-source`, `generate`, `validate`, `build-pack`, `publish-pack`.
- Implement orchestration runner for end-to-end demo flow.
- Implement error classification and remediation messaging.

### Filesystem & Storage

- Implement canonical filesystem layout (`staging/`, `packs/`, `logs/`).
- Implement atomic moves and checksum computation utilities.

### Testing & Contracts

- Implement contract test suites for manifest schema validation.
- Implement contract tests for event payload shapes (conceptual).
- Create end-to-end test pack demonstrating full pipeline.

### Documentation

- Document all APIs and CLI interfaces.
- Document configuration schema.
- Document filesystem layout.
- Create step-by-step guide for running demo pack build.

---

## KC-2 — Service Hardening & Automation

### Containerization & CI/CD

- Containerize all services (Generator, Validator, Publisher, Registry).
- Implement CI/CD pipelines (build, test, integration tests, artifact storage).
- Implement automated deployment to staging environment.

### Observability & Telemetry

- Implement telemetry collection (metrics, logs, traces).
- Implement SLO/SLA monitoring and alerting.
- Implement debug traces and feature flags.
- Define and document operational runbooks for key failures.

### Event Bus & Orchestration

- Choose and integrate durable event bus (Kafka, Pub/Sub, etc.).
- Implement event serialization and schema validation.
- Implement orchestration state machine (generator → validator → builder → publisher).
- Implement idempotency and retry logic for event consumers.

### Key Management & Signing

- Implement registry-backed signer key discovery.
- Implement prototype HSM integration for signing keys.
- Implement key rotation and ephemeral signing token generation.
- Implement signature verification tooling for consumers.

### Authentication & RBAC

- Implement SSO/OIDC integration points.
- Implement role-based access control for CLI and APIs.
- Implement role templates (author, validator, publisher, admin).

### Configuration Management

- Implement environment-agnostic configuration loading.
- Implement feature flags and deployment gates.

### Backup, Restore & Disaster Recovery

- Implement automated registry snapshots and backups.
- Define RPO/RTO objectives and test procedures.
- Implement cross-region replication for critical registries.

---

## KC-3 — Enterprise Features & Marketplace Foundations

### Multi-tenancy & Isolation

- Implement multi-tenant registry design with logical partition support.
- Implement per-tenant quotas and rate limiting.
- Implement per-tenant key encryption and separation.
- Implement tenant lifecycle management (create, suspend, deprovision).

### Monetization & Entitlements

- Implement monetization metadata in manifest (pricing, entitlements).
- Implement entitlement enforcement in Publisher (pack channel promotion).
- Implement billing integration hooks and ledger logging.
- Implement license metadata validation in Validator.

### Marketplace & Discovery

- Implement discovery APIs for pack catalog search and filtering.
- Implement marketplace UI (pack details, reviews, ratings, licensing).
- Implement purchase flow stub (shopping cart, order placement).
- Implement entitlement check for pack retrieval.

### Client SDKs

- Implement Pack Consumer SDK (credential, pack download, verification, unpacking).
- Implement validation tools for consumers to verify pack integrity before use.

### Compliance & Governance

- Implement region tagging and residency enforcement.
- Implement data retention policies and lifecycle management.
- Implement audit logging and SIEM integration hooks.
- Implement export control and legal metadata fields.

---

## KC-4 — Production & Commercial Launch

### Global Distribution

- Implement CDN integration for pack distribution.
- Implement global registry replicas and consistency protocols.
- Implement traffic routing and failover.

### Enterprise Onboarding

- Implement customer provisioning workflows.
- Implement dedicated account management and SLA negotiation.
- Implement custom role and policy templates for enterprises.

### Production Hardening

- Implement security audit and penetration testing.
- Implement production monitoring, incident response, and escalation.
- Implement customer-facing dashboards and reporting.

### Legal & Compliance

- Complete legal review (ToS, Privacy, Data Processing, Licensing).
- Implement GDPR compliance and data subject request handling.
- Implement SOC2 and industry-specific compliance (HIPAA, PCI, etc.).

### Marketplace Operations

- Launch public marketplace with featured packs.
- Implement marketplace moderation and dispute resolution.
- Implement revenue sharing and publisher payouts.

---

## Backlog Organization

Items are organized by phase and component. Within each phase, work items are prerequisite-ordered (dependencies listed where applicable). Backlog items are not yet estimated or scheduled into sprints — estimations occur during sprint planning.

See `SPRINT_BOARD.md` for active sprint tracking and `MILESTONES.md` for phase-level schedules.
