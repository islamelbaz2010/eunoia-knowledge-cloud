# ADR Template — Eunoia Knowledge Cloud

**Title of Record:** [Use present tense, indicate the decision being made]

**Status:** [Proposed | Accepted | Deprecated | Superseded by ADR-XXXX]

**Context:**
[Describe the architectural issue that led to this decision. Include relevant constraints, requirements, and background.]

**Problem:**
[State the problem clearly. What architectural challenge or question does this ADR address?]

**Decision:**
[State the decision clearly. What is the chosen solution?]

**Rationale:**
[Explain why this decision was chosen over alternatives. Address trade-offs and implications.]

**Alternatives Considered:**
[List 2-3 alternatives evaluated and why they were rejected.]

**Consequences:**
[Describe the positive and negative consequences of this decision.]

**Implementation Notes:**
[Provide guidance for implementers: when does this apply, are there exceptions, what's the rollout plan?]

**Related Decisions:**
[Link to related ADRs or architecture documents.]

**Approval:**
- Architecture Board: [Date & Approver Name]
- Affected Teams: [Sign-off list]

**Supersedes:**
[List any prior ADRs this decision replaces.]

---

## ADR Lifecycle

1. **Proposed:** ADR submitted for review.
2. **Accepted:** Architecture Board approved; implementation may proceed.
3. **Deprecated:** ADR no longer recommended but may still be in use.
4. **Superseded:** Replaced by a newer ADR; link to successor.

## Submission Process

1. Create ADR in `docs/architecture/adr/ADR-XXXX-title.md` using this template.
2. Submit for Architecture Board review (email or PR).
3. Address feedback and update ADR.
4. Board approves and sends approval memo.
5. Increment `ARCHITECTURE_VERSION.md` (MAJOR, MINOR, or PATCH as appropriate).
6. Announce decision to affected teams.

## Versioning

- Each ADR is identified by a unique incrementing number: ADR-0001, ADR-0002, etc.
- Multiple ADRs can be proposed in a single review cycle, but each must be evaluated independently.
- Versioning of ARCHITECTURE_VERSION.md happens once per ADR cycle (not per ADR).

