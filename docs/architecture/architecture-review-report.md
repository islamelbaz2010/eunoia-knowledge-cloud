# Architecture Review Report — EKC (KC-0 review)

Date: 2026-07-03

Reviewer: Chief Software Architect (automated review assistant)

Summary
- I reviewed all architecture documents produced for EKC (KC-0). The design is solid and aligns with the stated goals (determinism, auditability, and marketplace readiness). The review identified several areas requiring clarification or strengthening to reduce technical debt and to support enterprise and commercial scenarios.

Findings (categorized)

1) Scalability Risks
- Registry growth: pack catalogs and taxonomy size may exceed single-node assumptions; index and sharding strategy unspecified.
- Event bus & orchestration: transport unspecified — choices (Kafka, Pub/Sub) have operational trade-offs not captured.
- Validation compute: deep validation (similarity, deduplication) can be costly; risk of noisy neighbors.

2) Monetization Limitations
- No concrete manifest fields for pricing, entitlements, or licensing enforcement initially — marketplace work requires explicit schema.
- No billing integration hooks or entitlement checks in Publisher flows defined.

3) Hidden Complexity
- Deterministic templating and sandboxing: safe, deterministic rendering of templates is non-trivial and requires strong runtime limits and deterministic libraries.
- Duplicate detection and similarity heuristics at scale (without ML) is difficult and computationally expensive.
- Key lifecycle and signature discovery across distributed registry replicas is operationally tricky.

4) Duplicated / Unclear Responsibilities
- Registry vs specific registries: overlap is expected but adapter patterns and clear owner responsibilities needed.
- Audit vs Publisher vs Registry: need explicit decision on where tamper-evident audit logs are stored and retained.

5) Missing Enterprise Features
- Identity and access: SSO/OIDC integration, role mapping, ABAC, per-tenant policies.
- Compliance: region tagging, retention, export controls, and legal metadata.
- Operations: backup/restore, DR, SLA/SLO definitions, metrics & alerting, runbooks.

6) Future Maintenance Risks
- Manifest extension misuse: arbitrary extensions can fragment consumer behavior.
- Versioning discipline: many moving versioned artifacts require strict compatibility rules and contract tests.
- Taxonomy governance: poor processes will harm discovery and increase technical debt.

Actions Taken (documents updated)
- Added enterprise & security guidance and SLO/observability notes to `01-system-overview.md`.
- Extended `02-layered-architecture.md` with Observability and Enterprise Operations layers.
- Added `Audit Logging`, `RBAC & Identity`, and `Telemetry` components to `03-component-specification.md`.
- Expanded `05-registry-design.md` with scaling, migration adapter, multi-tenant isolation, and backup/restore guidance.
- Extended `07-knowledge-pack-specification.md` with monetization and licensing fields, signer discovery, and retention metadata.
- Added operational guidance for validation compute costs and ruleset governance to `09-validation-specification.md`.
- Added key discovery, signing governance, and entitlement enforcement to `10-publisher-specification.md`.
- Added key discovery mechanism, extended error codes, and consumer operational notes to `11-ai-os-contract.md`.
- Added immediate milestones for contract tests, key management, and monetization schema to `12-roadmap.md`.
- Created this `architecture-review-report.md` summarizing the review and the edits applied.

Recommendations (prioritized)

Immediate (KC-1 / before reference implementation)
- Define and implement contract tests for manifest, event payloads, and registry shapes (prevent drift).
- Finalize minimal monetization schema and where entitlements are enforced (Publisher + Registry).
- Define signer key discovery contract (registry-backed metadata) and stub key lifecycle for KC-1.

Short-term (KC-2)
- Choose durable event transport and design adapters; document operational trade-offs.
- Implement registry partitioning and migration adapter patterns.
- Implement telemetry, SLOs, and backup/restore procedures.

Mid-term (KC-3)
- Implement multi-tenant tenancy model, per-tenant quotas, and billing integrations.
- Add marketplace UX/API and entitlement enforcement paths.

Long-term (KC-4)
- Harden global distribution, multi-region replication, and enterprise legal/compliance workflows.

Open Architectural Questions (need stakeholder input)
- Which event transport do we standardize on for KC-2 (self-managed Kafka vs managed Pub/Sub vs Kinesis)?
- What is the commercial model (per-pack pricing, subscription, usage-based)? This determines manifest monetization fields.
- Tenant isolation policy: strict logical isolation vs physical separation for high-security customers?

Risks & Mitigations
- Risk: Early coupling to a datastore/event bus. Mitigate: enforce adapter interfaces and contract tests.
- Risk: Validation compute cost spikes. Mitigate: validation tiers, quotas, and async deep-check queues.
- Risk: Key compromise. Mitigate: HSM-backed keys in KC-2 and rotation policies; minimal privilege for signing tokens.

Next Steps
- Review and approve these doc updates with architecture board.
- Prioritize contract tests, signer key design, and monetization schema in KC-1 planning.

---
_End of report._
