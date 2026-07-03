# Architecture Freeze Record — Eunoia Knowledge Cloud

Architecture Version: 0.1.0 (KC-0 freeze)

Freeze Date: 2026-07-03T00:00:00Z

Change Policy
- From the Freeze Date onward, no architectural changes to the documents under `docs/architecture/` are permitted without a recorded Architecture Decision Record (ADR).
- An ADR must document: title, author, motivation, alternatives considered, decision, implications, and roll-back plan.
- ADRs must be reviewed and approved by the Architecture Board (or designated reviewers) and linked into the Decision Log in this file when accepted.
- Emergency changes are allowed only under documented incident procedures and must be followed by a retrospective ADR within 3 business days.

Versioning Rules
- Architecture versions follow semantic versioning with the following meanings:
  - MAJOR (X.0.0): incompatible architectural changes that require consumers or implementers to change their implementations or contracts.
  - MINOR (0.X.0): additive, backwards-compatible architecture extensions (e.g., new optional manifest fields, new events that are optional for consumers).
  - PATCH (0.0.X): editorial fixes, clarifications, or non-behavioral updates that do not change contracts or expectations.
- Only ADR-approved MAJOR or MINOR changes cause a version bump; editorial updates may increment PATCH with Architecture Board sign-off.
- Each ADR must specify the target architecture version and the required version increment, and the Architecture Board must approve the increment.

Decision Log
- 2026-07-03 — KC-0 architecture freeze (this file). Author: Chief Software Architect.
- 2026-07-03 — Event-driven integration chosen as canonical integration fabric. Decision: publish event catalog and require idempotent consumers. Rationale: decoupling and scalability. (See `docs/architecture/04-event-driven-architecture.md`.)
- 2026-07-03 — Registry-first model adopted: authoritative registries for Source, Dataset, Pack, Template, Schema, Taxonomy, Version. Rationale: deterministic generation and discovery.
- 2026-07-03 — Knowledge Pack format (manifest-first, checksums, detached signature) approved as canonical pack artifact. (See `docs/architecture/07-knowledge-pack-specification.md`.)
- 2026-07-03 — KC-0 scope restrictions: NO connectors, NO crawlers, NO AI, NO embeddings, NO vector search, NO Supabase, NO SaaS APIs, NO databases implemented in KC-0. Rationale: narrow architecture sprint focus.
- 2026-07-03 — Channels: `draft`, `staged`, `stable` publication channels approved. (See `docs/architecture/10-publisher-specification.md`.)
- 2026-07-03 — Registry-backed signer key discovery mandated for KC-1 as initial trust model. (See `docs/architecture/07-knowledge-pack-specification.md` and `docs/architecture/11-ai-os-contract.md`.)
- 2026-07-03 — Monetization extension fields added to manifest for marketplace readiness: `manifest.monetization` and `manifest.license`. (See `docs/architecture/07-knowledge-pack-specification.md`.)
- 2026-07-03 — Validation tiers (Strict, Standard, Advisory) approved to balance throughput and quality. (See `docs/architecture/09-validation-specification.md`.)

Definition of Done — Architecture Sprint KC-0
- All required architecture documents exist under `docs/architecture/` and reflect the project's core design decisions.
- Clear system boundaries established and KC-0 exclusions documented.
- Event catalog defined and event payload shapes specified at a conceptual level.
- Knowledge Pack format, manifest, checksums, and signing contract defined.
- Validation, registry, generator, and publisher designs documented with responsibilities, interfaces, and error models.
- Architecture Review performed and recorded; immediate remediation edits applied (see `architecture-review-report.md`).
- Architecture Version file (this file) created and freeze recorded.

Post-freeze governance
- Any proposed change to the architecture must be submitted as an ADR referencing this file and the affected documents.
- The Architecture Board will review ADRs, approve or reject changes, and, if approved, update `ARCHITECTURE_VERSION.md` with the new version number and append the decision to the Decision Log.

No further architectural changes will be accepted into the KC-0 documents without an ADR and Architecture Board approval.
