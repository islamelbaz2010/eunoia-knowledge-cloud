# ADR-0003 — Generator Pipeline & Deterministic Artifact Production

**Status:** Accepted

**Context:**
The Generator is EKC's core production system, consuming sources, datasets, and templates to produce Knowledge Pack artifacts. For audit compliance, reproducibility, and consumer trust, artifacts must be deterministically reproducible: re-running the generator with identical inputs and versions must yield byte-identical output.

**Problem:**
Nondeterministic rendering (due to floating-point rounding, library version differences, environment-specific behavior, or randomization) prevents reproducibility and breaks audit trails. A generator design that guarantees determinism is required.

**Decision:**
Implement the Generator as a deterministic, stage-based pipeline:
1. Resolve Inputs: fetch pinned versions from registries.
2. Pre-flight Validation: schema checks, taxonomy verification.
3. Transform & Render: deterministic templating with sandboxed, time-limited execution.
4. Staging: write artifacts to canonical path with checksums.
5. Post-generation Validation: run Validator with versioned rulesets.
6. Emit DatasetGenerated event.

Each run produces an immutable runId linking to exact registry versions, config snapshot, and template digests, enabling perfect reproduction.

**Rationale:**
- **Reproducibility:** Byte-identical output enables debugging, audits, and rollback.
- **Determinism mechanisms:** Pin all library versions, use deterministic templating languages, avoid randomization and floating-point ops.
- **Stage-based:** Clear separation of concerns, enabling retry logic and independent testing.
- **Sandboxing:** Prevent template-based exploits (e.g., arbitrary file access, network calls).

**Alternatives Considered:**
1. **Stateful, incremental generation:** Complex to debug, harder to guarantee determinism. Rejected.
2. **Non-sandboxed templating:** Security risk (templates can access environment, network). Rejected.
3. **Deterministic-as-goal (not requirement):** Weakens audit and reproducibility guarantees. Rejected.
4. **Stage-based deterministic pipeline** (chosen): Clear boundaries, testable, auditable.

**Consequences:**
- ✅ Audit compliance: reproducible, auditable artifact generation.
- ✅ Consumer trust: consumers can independently verify pack provenance.
- ✅ Debugging: identical re-runs enable root cause analysis.
- ⚠️ Runtime overhead: sandboxing and pinning versions adds compute cost.
- ⚠️ Template complexity: deterministic templating languages have limitations (no external I/O).
- ⚠️ Implementation discipline: must carefully manage library versions and environment setup.

**Implementation Notes:**
- Generator must document all pinned versions (runtime, libraries, templates).
- Determinism tests in KC-1: run generator twice with same inputs, verify byte-identical output.
- Template rendering engine must be chosen carefully (e.g., Jinja2 with frozen loader, or custom DSL).
- Sandboxing: limit file access to staging area, disable network and subprocess creation, enforce timeout (e.g., 30 second max per template).
- Config snapshot: capture generator config, template versions, and registry state at generation time.
- Error classification: distinguish Fatal (human remediation) vs Retryable (transient resource) errors, provide remediation hints.

**Related Decisions:**
- ADR-0004 (Registry Strategy): versioning of templates and schemas enables reproducibility.
- Contract: `docs/architecture/06-generator-specification.md`.

**Approval:**
- Architecture Board: Approved 2026-07-03
- Document: ARCHITECTURE_VERSION.md, docs/architecture/06-generator-specification.md

**Supersedes:**
None (first specification of generator design).
