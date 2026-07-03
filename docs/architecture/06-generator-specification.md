# Generator Specification

This document defines the Knowledge Pack Generator pipeline in detail: stages, inputs, outputs, validation, error handling, and versioning.

## Goals

- Deterministic output for reproducibility.
- Stage-based pipeline with clear handoffs and observability.
- Robust error classification and remediation guidance.

## Pipeline Stages

1. Resolve Inputs
2. Pre-flight Validation
3. Transform & Render
4. Staging
5. Post-generation Validation
6. Emit `DatasetGenerated`

### 1. Resolve Inputs

- Inputs: `sourceRef`, `datasetRef`, `templateRef`, `schemaRef`, `config`.
- Actions: fetch metadata from registries, materialize dataset pointers.
- Output: resolved inputs and `resolveReport`.
- Errors: missing references, incompatible versions.

### 2. Pre-flight Validation

- Run schema checks on metadata, ensure templates exist, verify taxonomy mappings.
- Output: `preflightReport` with pass/fail and warnings.

### 3. Transform & Render

- Execute deterministic template rendering and transforms producing artifacts (markdown, JSON-LD, assets).
- Ensure sandboxed rendering environment with resource/time limits.

### 4. Staging

- Write artifacts to a canonical staging path: `staging/<sourceId>/<runId>/`.
- Compute checksums for each artifact and a manifest draft.

### 5. Post-generation Validation

- Invoke Validator to run deeper checks against schemas and quality rules. Produce `validationReport`.
- On `ValidationPassed`, pipeline proceeds to Pack Builder; on `ValidationFailed`, emit event and attach report.

### 6. Emit `DatasetGenerated`

- Emit event with references to staging path, manifest draft, and `validationReportRef`.

## Inputs & Outputs

- Inputs: Source metadata, dataset materials, templates, schema contracts, config.
- Outputs: Staged artifacts, generation logs, `GenerationReport`.

## Validation & Error Handling

- Errors are categorized as `Fatal` (stop, human remediation), `Retryable` (transient resources), and `Warn` (non-blocking quality issues).
- All failures produce structured diagnostics and suggested remediation steps.

## Versioning & Reproducibility

- Each generation run produces an immutable `runId` and records: exact registry versions, config snapshot, and template digests.
- Re-running with identical inputs and versions must yield byte-identical staged artifacts.

## Observability

- Emit metrics: generation duration, artifact counts, size, validation score.
- Attach `correlationId` and `traceId` to all logs and events.

## Security Considerations

- Renderers must sandbox untrusted templates and limit network/file access.

---
_Generator pipeline is the deterministic core of EKC, carefully instrumented to support audits and reproducibility._
