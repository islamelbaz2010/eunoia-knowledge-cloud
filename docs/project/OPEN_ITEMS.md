# Open Items & Blockers

Items awaiting decision or input before KC-1 implementation can proceed at full speed.

## Blocking Decisions

### 1. Event Bus Technology Selection

**Status:** OPEN (required for KC-2)

**Requirement:** Choose durable event transport (Kafka, AWS Kinesis, Google Pub/Sub, Azure Service Bus, RabbitMQ).

**Evaluation Criteria:**
- Throughput (events/sec) and latency (p99).
- Multi-region support and geo-failover.
- Operational complexity (self-managed vs managed).
- Cost at scale (millions of packs).
- Community & vendor support.

**Decision Deadline:** KC-1 completion (before KC-2 implementation).

**Owner:** Architecture Board + Platform Team.

---

### 2. Commercial Business Model

**Status:** OPEN (required for KC-2/KC-3)

**Requirement:** Define monetization model:
- Per-pack pricing (fixed, tiered, or usage).
- Subscription (free tier + paid tiers).
- Usage-based (storage, downloads, API calls).
- Hybrid (combination).

**Impact:** Determines manifest monetization fields, billing system design, and marketplace features.

**Decision Deadline:** Early KC-2 (before SDK & marketplace features locked in).

**Owner:** Product Team + Business Stakeholders.

---

### 3. Multi-tenant Isolation Policy

**Status:** OPEN (required for KC-3)

**Requirement:** Define tenant isolation constraints:
- Logical isolation (per-tenant database views with shared storage).
- Physical isolation (dedicated storage per tenant).
- Encryption (per-tenant keys, regional keys, or global keys).

**Impact:** Determines architecture for KC-3 marketplace and enterprise deployments.

**Decision Deadline:** KC-2 (before KC-3 multi-tenancy implementation).

**Owner:** Security Team + Product Team.

---

### 4. Key Management Strategy

**Status:** OPEN (required for KC-2)

**Requirement:** Specify signer key lifecycle:
- HSM vs software keys.
- Key rotation frequency and procedure.
- Ephemeral signing tokens for automation.
- Disaster recovery and key loss procedures.

**Impact:** Determines security posture, operational overhead, and costs.

**Decision Deadline:** KC-2 planning (before key management team begins work).

**Owner:** Security Team + Operations Team.

---

## Information Gaps

### 1. Compliance Requirements

**Status:** UNDER INVESTIGATION

- What regulations apply? (GDPR, CCPA, HIPAA, PCI, SOC2, ISO27001).
- What audit/logging requirements?
- What data residency constraints?
- What export controls?

**Action:** Engage legal and compliance consultants in KC-2 planning.

---

### 2. Market Research

**Status:** PENDING

- Target customer segments (individual creators, SMBs, enterprises).
- Willingness to pay (per-pack vs subscription pricing).
- Competitive landscape and differentiation points.
- Launch marketing strategy.

**Action:** Product/marketing team to conduct customer discovery in parallel with KC-1 implementation.

---

### 3. Integration Requirements

**Status:** PENDING

- Eunoia AI OS consumption patterns and expected SLA.
- Third-party integrations (billing, auth providers, CDNs).
- API rate limits and quota requirements.

**Action:** Coordination with AI OS team throughout KC-1/KC-2.

---

## Recommendations

- **KC-1 scope:** Proceed with reference implementation using assumptions (e.g., assume free model, logical multi-tenancy, local signing). Plan to update these decisions in KC-2.
- **KC-1 parallel:** Schedule stakeholder workshops in weeks 2-3 of KC-1 to resolve blocking decisions before KC-2 begins.
- **KC-2 early phase:** Incorporate decisions into service hardening and infrastructure choices.

---

## Tracker

| Item | Status | Owner | Deadline |
|------|--------|-------|----------|
| Event Bus | OPEN | Platform Team | End of KC-1 |
| Business Model | OPEN | Product/Business | Early KC-2 |
| Multi-tenancy Isolation | OPEN | Security/Product | End of KC-2 |
| Key Management | OPEN | Security Team | End of KC-2 |
| Compliance | INVESTIGATING | Legal Team | Start of KC-2 |
| Market Research | PENDING | Product/Marketing | Ongoing |
| AI OS Integration | PENDING | AI OS Liaison | Ongoing |
