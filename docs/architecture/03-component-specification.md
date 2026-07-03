# Component Specification

This document defines each major module in EKC, describing responsibilities, public interfaces, dependencies, outputs, errors, and extension points.

For every module below the sections are: Purpose, Responsibilities, Public Interfaces, Dependencies, Outputs, Errors, Future Extension Points.

---

## Generator

Purpose: Produce candidate Knowledge Pack artifacts deterministically from registered Sources, Templates, and Schemas.

Responsibilities:
- Read Source/Template/Schema metadata from registries.
- Run templating and deterministic transforms.
- Emit staged artifact trees into the Filesystem staging area.

Public Interfaces:
- `generate(sourceRef, version, config) -> GenerationReport` (synchronous request/response for CLI).
- Event: `DatasetGenerated` emitted on completion.

Dependencies: Template Registry, Schema Registry, Filesystem staging.

Outputs: staged artifact tree, generation logs, provenance metadata.

Errors: template resolution error, schema mismatch, missing assets, deterministic mismatch.

Future Extension Points: pluggable transform stages, parallel subgenerators, incremental generation.

---

## Publisher

Purpose: Sign, publish, and distribute Knowledge Packs to the Pack Registry and downstream distribution targets.

Responsibilities:
- Verify checksums and signatures on pack contents.
- Promote pack versions between channels (draft, staged, stable).
- Retain provenance and release notes.

Public Interfaces:
- `publish(packRef, channel, policy) -> PublishReceipt`.
- Event: `PackPublished`.

Dependencies: Pack Registry, Filesystem storage, signing key store.

Outputs: published pack record, distribution logs, signed manifest.

Errors: signature failure, integrity mismatch, policy violation.

Future Extension Points: CDN distribution, multi-region replication, release gating.

---

## Registry (generic)

Purpose: Authoritative index of named entities (Source, Dataset, Pack, Template, Schema, Taxonomy, Version).

Responsibilities:
- Provide strong, versioned references and discovery semantics.
- Enforce uniqueness and basic schema validation for metadata.

Public Interfaces:
- `register(entity)`, `get(entityRef)`, `list(filters)`, `deprecate(entityRef)`.

Dependencies: Filesystem-backed persistence (KC-0), later a distributed database.

Outputs: resolvable entity references and metadata documents.

Errors: duplicate registration, invalid metadata, referential integrity failures.

Future Extension Points: fine-grained ACLs, multi-tenant shards, query APIs.

---

## Validator

Purpose: Enforce policy, quality, and compatibility checks across candidate artifacts.

Responsibilities:
- Run syntactic and semantic checks against schemas.
- Run quality heuristics and domain-specific rules.
- Produce graded validation reports (errors, warnings, informational).

Public Interfaces:
- `validate(stagingPath, ruleset) -> ValidationReport`.
- Event: `ValidationPassed` or `ValidationFailed`.

Dependencies: Schema Registry, Taxonomy, Rules engine configuration.

Outputs: ValidationReport JSON with detailed failure telemetry.

Errors: unexpected runtime exceptions, rule engine timeout, unsupported asset type.

Future Extension Points: ML-assisted validation hints, configurable rules DSL.

---

## Taxonomy

Purpose: Centralized ontological model for country, industry, language, content-type, and tags used for classification and discovery.

Responsibilities:
- Provide canonical identifiers and controlled vocabulary.
- Support hierarchical relationships and mappings.

Public Interfaces:
- `resolve(term)`, `expand(term)`, `mapTo(country|industry)`.

Dependencies: Registry, Schema definitions.

Outputs: taxonomy payloads and normalize() utilities.

Errors: invalid term, conflicting mappings.

Future Extension Points: taxonomy ownership model, relationship versioning.

---

## Manifest

Purpose: Pack-level manifest describing contents, versioning, provenance, compatibility, and checksums.

Responsibilities:
- Define canonical manifest schema and validation rules.
- Provide manifest generation utilities used by the pack builder.

Public Interfaces:
- `createManifest(packContents, metadata) -> manifest.json`.

Dependencies: Validator, Registry, Filesystem.

Outputs: `manifest.json`, `manifest.sig`.

Errors: missing fields, incompatible version constraints.

Future Extension Points: manifest extensions for monetization metadata, licensing.

---

## Dataset

Purpose: Logical unit of curated content used as input to generation.

Responsibilities:
- Define dataset metadata, provenance, and versioning.
- Provide dataset resolution for generator tasks.

Public Interfaces:
- `registerDataset(metadata)`, `getDataset(ref)`.

Dependencies: Source Registry, Filesystem.

Outputs: dataset pointers and resolved content references.

Errors: invalid dataset schema, missing source assets.

Future Extension Points: dataset diffing, derived datasets, canonical snapshots.

---

## Template

Purpose: Reusable templates and transformation rules that determine pack content structure and rendering.

Responsibilities:
- Store versioned templates and rendering instructions.
- Expose safe rendering primitives for deterministic generation.

Public Interfaces:
- `render(templateRef, data) -> artifact`.

Dependencies: Registry, Generator.

Outputs: rendered artifacts consumed by Validator and Pack Builder.

Errors: template runtime errors, missing partials.

Future Extension Points: pluggable template engines, compiled template caching.

---

## Filesystem

Purpose: Canonical organization for temporary staging and final artifact storage.

Responsibilities:
- Define canonical paths for `staging/`, `packs/`, `temp/`, `logs/`.
- Provide utilities for atomic moves and checksum computation.

Public Interfaces:
- `stagePath(packId)`, `finalizePack(stagingPath) -> packPath`.

Dependencies: underlying object store or POSIX filesystem.

Outputs: immutable pack blobs and provenance logs.

Errors: IO errors, insufficient disk space, permission errors.

Future Extension Points: pluggable storage backends, lifecycle policies.

---

## Configuration

Purpose: Centralized, versioned runtime configuration for rulesets, policies, and service endpoints.

Responsibilities:
- Provide environment-agnostic configurations and feature flags.

Public Interfaces:
- `getConfig(namespace, key)`.

Dependencies: Registry for versioned config in KC-0.

Outputs: resolved configuration values.

Errors: missing configuration, invalid config schema.

Future Extension Points: hierarchical overrides, per-tenant config.

---

## Audit Logging (cross-cutting)

Purpose: Provide immutable operational audit trails for all actions affecting registry entries, validation runs, and publishing events.

Responsibilities:
- Capture actor, timestamp, action, targetRef, and before/after deltas.
- Store tamper-evident logs linked to events and manifests.

Public Interfaces:
- `appendAudit(record)`, `queryAudit(targetRef, timeRange)`.

Dependencies: Registry, Filesystem/secure log store.

Errors: failure to persist audit entry must block high-sensitivity operations until resolved.

Future Extension Points: integration with SIEM and WORM storage.

---

## RBAC & Identity

Purpose: Define roles, permissions, and identity mapping used by EKC components and UI/CLI.

Responsibilities:
- Define role templates (author, validator, publisher, admin) and fine-grained permissions for registry operations.
- Provide hooks for SSO/OAuth/OIDC providers.

Public Interfaces:
- `authorize(actor, action, resource) -> decision`.

Dependencies: Configuration, Registry (for role bindings).

Errors: unauthorized attempts must produce audit entries and clear error codes.

Future Extension Points: attribute-based access control (ABAC) and per-tenant role overrides.

---

## Telemetry

Purpose: Provide structured metrics and traces for pipelines, registries, validation runs, and publishing.

Responsibilities:
- Emit standard metrics (latency, counts, error rates) and traces with `correlationId`.
- Provide diagnostic hooks for sampling, feature flags, and debug traces.

Public Interfaces:
- `emitMetric(namespace, metric, value, tags)`, `startSpan(name, tags)`.

Dependencies: Infrastructure telemetry backends (implementation-specific).

Errors: telemetry failures must not block primary flows but must be logged.

Future Extension Points: automated anomaly detection and SLO enforcement.
