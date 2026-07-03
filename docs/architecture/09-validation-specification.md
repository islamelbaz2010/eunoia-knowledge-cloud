# Validation Specification

Validation ensures Knowledge Packs meet syntactic, semantic, policy, and quality requirements before publication.

## Validation Pipeline Stages

1. Schema Validation
2. Policy & Compliance Checks
3. Quality Heuristics
4. Deduplication & Similarity Checks
5. Final Gate (pass/fail thresholds)

### Quality Levels

- `Strict` — required for production/stable channel; zero critical errors; high-quality thresholds.
- `Standard` — acceptable for staged channel; warnings allowed.
- `Advisory` — diagnostics only; does not block generation.

### Checks and Rules

- Syntactic: JSON/Schema conformance, manifest completeness.
- Semantic: cross-field consistency, taxonomy mappings.
- Policy: licensing, allowed content types, PII detection (static heuristics only — no AI in KC-0).
- Quality heuristics: minimum content length, link integrity, image alt-text presence.
- Deduplication: artifact-level checksum collisions and content similarity detection (non-AI heuristics).

### Duplicate Detection

- Primary: exact checksum match (reject or mark duplicate).
- Secondary: structural similarity heuristics (flag as potential duplicate for human review).

### Reporting Model

- ValidationReport contains: `score`, `errors[]`, `warnings[]`, `info[]`, `remediationHints[]`.
- Errors have severity levels (`critical`, `major`, `minor`) and canonical error codes.

### Error Handling

- Critical errors block publication; remediation required.
- Major errors require explicit approval to proceed (manual override path documented in governance).
- Minor errors are recorded as warnings and may be auto-fixed where safe (e.g., missing `README.md` template can be injected).

### Automation & Fixes

- Non-invasive auto-fixes permitted for deterministic fixes (e.g., normalize metadata fields).
- All auto-fixes must be recorded in the ValidationReport with before/after diffs.

### Audit and Traceability

- Validation runs are logged with `validatorVersion`, `rulesetVersion`, and evidence attachments.

---
_Validation is the principal quality gate. Rulesets and thresholds are versioned and governed._

## Operational Considerations

- **Compute cost & throttling:** Validation can be compute-intensive (large packs, dedup checks). Implement resource budgets, parallelism limits, and tenant-level quotas to avoid noisy-neighbor issues.
- **Offload & tiering:** Introduce validation tiers (fast checks vs deep checks) to allow acceptable turnaround for high-throughput flows while queuing deep validation for asynchronous processing.

## Governance & Rulesets

- **Ruleset versioning:** Rulesets are versioned and stored in the Schema Registry. Changes to rules require staged rollout and can be feature-flagged.
- **Approval workflows:** Major validation rules that block publishing must support governance workflows for manual override with audit trails.
