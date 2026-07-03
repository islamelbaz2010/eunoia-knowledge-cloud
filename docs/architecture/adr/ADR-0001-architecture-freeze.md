# ADR-0001 — Architecture Freeze and Change Governance

**Status:** Accepted

**Context:**
EKC has completed its architecture sprint (KC-0) with comprehensive design documentation covering system overview, layered architecture, component specifications, event-driven integration, registries, generator pipeline, knowledge pack format, taxonomy, validation, publisher, consumer contract, and roadmap. To ensure architectural stability and enable parallel implementation work across teams, a freeze mechanism is required.

**Problem:**
Without a change governance process, architectural decisions can drift during implementation, causing incompatibilities between components and with consumers. Continuous changes also increase overhead for teams trying to plan and schedule work. A formal mechanism is needed to: (1) freeze the current architecture, (2) define how changes are approved, and (3) version architecture changes for consumers.

**Decision:**
Freeze the architecture at version 0.1.0 as of 2026-07-03. All future architectural changes require an Architecture Decision Record (ADR) approved by the Architecture Board. Changes are categorized by impact (MAJOR, MINOR, PATCH) and reflected in `ARCHITECTURE_VERSION.md` and the Architecture Board decision log.

**Rationale:**
- **Stability:** Frozen architecture provides a stable contract for implementation teams.
- **Governance:** ADRs force explicit evaluation of changes and document rationale.
- **Versioning:** Semantic versioning allows consumers to understand compatibility implications.
- **Traceability:** Decision log creates audit trail for architectural evolution.

**Alternatives Considered:**
1. **No freeze, continuous evolution:** Risk of architectural drift and implementation churn. Rejected.
2. **Freeze without ADR process:** Changes could be made secretly or inconsistently. Rejected.
3. **Freeze with single approver:** Concentrates risk. **Chosen governance** requires Board review (not a single person).

**Consequences:**
- ✅ Architectural stability and predictable contracts for implementers and consumers.
- ✅ Clear audit trail of why architectural decisions were made.
- ✅ Sets precedent for disciplined architectural evolution.
- ⚠️ Slows emergency changes (but emergency path is documented).
- ⚠️ Requires Architecture Board overhead for ADR reviews.

**Implementation Notes:**
- Architecture Board is responsible for ADR review and approval.
- Emergency changes are allowed under documented incident procedures; retrospective ADRs required within 3 business days.
- ADRs must be submitted using the template in `adr/ADR_TEMPLATE.md`.
- ARCHITECTURE_VERSION.md is the source of truth for approved changes and current version.

**Related Decisions:**
- ADR-0002 (Knowledge Pack Contract): specific architectural decision subject to freeze.
- ADR-0003 (Generator Pipeline): specific architectural decision subject to freeze.
- ADR-0004 (Registry Strategy): specific architectural decision subject to freeze.

**Approval:**
- Architecture Board: Approved 2026-07-03
- Document: ARCHITECTURE_VERSION.md

**Supersedes:**
None (first ADR).
