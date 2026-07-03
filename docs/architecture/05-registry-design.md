# Registry Design

The Registry layer provides authoritative indices for entities used across EKC. Registries store versioned metadata, enforce identity and referential integrity, and provide discovery.

## Registry Types

- Connector Registry (out of scope for KC-0) — records connector definitions and capabilities.
- Source Registry — canonical list of registered knowledge sources and their manifests.
- Dataset Registry — logical datasets and derived snapshots.
- Pack Registry — published Knowledge Packs and versions; primary consumer API for Eunoia AI OS.
- Template Registry — versioned templates and render instructions.
- Schema Registry — canonical schemas for manifests, metadata, and validation rules.
- Taxonomy Registry — controlled vocabulary and mappings.
- Version Registry — time-series index of entity versions for audit and rollback.

## Core Design Principles

- Versioned metadata: all registry entities are versioned with semantic versioning rules where applicable.
- Referential integrity: registry entries reference other entities by stable ids and versions.
- Immutable history: prior versions remain accessible for audit and rollback.
- Lightweight findability: registries support list and filter operations for discovery.

## Entity Model (common fields)

- `id`: stable unique identifier.
- `type`: entity type (Source, Dataset, Pack, Template, Schema, Taxonomy).
- `version`: semantic version or snapshot timestamp.
- `metadata`: structured metadata JSON.
- `provenance`: pointers to author, creation event, generation inputs.

## Registry Operations

- `register(entity)` — validate and persist a new version.
- `get(id, version?)` — fetch latest or specific version.
- `list(filter, pageToken)` — discovery.
- `deprecate(id, version?)` — mark version(s) as deprecated.

## Consistency Model

- Registries must provide read-after-write semantics for internal flows; eventual consistency across read replicas is acceptable for external consumers.

## Access & Governance

- Registries enforce role-based access control for registration and deprecation operations.

## Storage & Scaling (KC-0 guidance)

- KC-0 defines registry schemas and file-backed proof-of-concept implementations. Later phases will map to a distributed datastore. APIs must be designed to allow migration with minimal contract changes.

---
_Registry design provides the durable naming and discovery foundation required by all other modules._

## Production Scaling & Migration Guidance

- **Partitioning:** design registries with namespace partitioning (by tenant, region, or content domain) and clear sharding keys for large catalogs.
- **Indexes & search:** separate hot-path indices (by packId, version, taxonomy tags) from archival storage; provide secondary indexes for discovery.
- **Migration adapter:** define a storage adapter interface and migration plan so KC-1 file-backed registries can be replaced by distributed datastores with minimal contract changes.

## Multi-tenant & Isolation Modes

- Support single-tenant and multi-tenant registry modes. Provide policy-controlled isolation (logical separation, encryption keys per tenant, and optionally per-tenant storage backends) for enterprise compliance.

## Backup, Restore & Retention

- Registries must include automated snapshot, backup, and restore procedures, with documented RPO/RTO objectives and regional replication strategies.

