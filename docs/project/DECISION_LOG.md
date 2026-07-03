# Architecture Decision Log (ADL)

Decisions recorded during KC-0 and linking to ADRs. (Note: ADRs are the authoritative record; this log is a quick reference.)

## KC-0 Decisions

| Date | Decision | ADR | Status |
|------|----------|-----|--------|
| 2026-07-03 | Architecture Freeze: KC-0 complete, versioned 0.1.0. No further architectural changes without ADR. | ADR-0001 | ✅ Approved |
| 2026-07-03 | Knowledge Pack Contract: manifest-based pack format with detached signatures, checksums, and provenance. | ADR-0002 | ✅ Approved |
| 2026-07-03 | Generator Pipeline: deterministic, templated artifact generation with stage-based validation. | ADR-0003 | ✅ Approved |
| 2026-07-03 | Registry Strategy: file-backed proof-of-concept in KC-1, migration adapter for future datastores, multi-tenant support in KC-3. | ADR-0004 | ✅ Approved |

## Rationale Summary

**Architecture Freeze (ADR-0001):**
- Ensures stable contracts for implementations.
- Future changes require formal review and versioning.
- Minimizes scope creep and mid-sprint design churn.

**Knowledge Pack Contract (ADR-0002):**
- Self-contained, checksummed packs enable independent verification.
- Detached signatures support post-hoc verification without requiring pack modification.
- Manifest schema enables marketplace and compliance metadata.

**Generator Pipeline (ADR-0003):**
- Deterministic rendering enables reproducibility and audit.
- Stage-based pipeline provides clear error handling and observability points.
- Sandboxed templates prevent template-based exploits.

**Registry Strategy (ADR-0004):**
- File-backed initial implementation avoids early infrastructure commitment.
- Migration adapter prevents vendor lock-in and enables polyglot database choices.
- Multi-tenant design in KC-3 supports commercial marketplace and enterprise isolation.

## Ongoing Decisions (deferred to KC-2)

- Event bus technology selection (Kafka, Pub/Sub, Kinesis).
- Commercial business model and pricing strategy.
- Multi-tenant isolation policy and enforcement.
- HSM vs software-based key management.

See `RISKS.md` and `OPEN_ITEMS.md` for additional items awaiting stakeholder input.
