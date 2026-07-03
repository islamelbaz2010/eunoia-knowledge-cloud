# Roadmap — KC-0 through KC-4

This roadmap defines phased work to move EKC from architecture sprint (KC-0) to production-grade platform and commercial productization.

## Phases

### KC-0 — Architecture & Specification (current)

- Deliverables:
  - Complete architecture documentation (this directory).
  - Formalized event and registry contracts.
  - Pack format and validation rules specified.
- Definition of Done:
  - Documentation approved by architecture board.
  - Contract tests (specs) created for registries and pack manifest.
  - No runnable implementations included in repository.

### KC-1 — Reference Implementation (proof-of-concept)

- Deliverables:
  - Minimal reference generator, validator, and registry implemented as modular CLI tools.
  - File-backed registries and filesystem storage.
  - End-to-end pipeline for building and publishing a demo pack.
- Dependencies: None external; no AI or connectors used.

### KC-2 — Service Hardening & Automation

- Deliverables:
  - Containerized services, CICD pipelines, key management prototype.
  - Event bus implementation and orchestration.
  - Authentication, basic RBAC, and telemetry.
- Dependencies: Infra provisioning (object storage), CI/CD.

### KC-3 — Enterprise Features & Marketplace Foundations

- Deliverables:
  - Multi-tenant registry design and tenancy prototype.
  - Monetization metadata in manifest, purchase flow stubs.
  - Client SDKs and consumption contract validation tools.
- Dependencies: Billing & payments integration, legal review.

### KC-4 — Production & Commercial Launch

- Deliverables:
  - Scalable distributed registries, global distribution.
  - Marketplace UI and developer portal.
  - SLA, compliance, and enterprise onboarding processes.

## Deliverables (summary)

- Complete documentation (KC-0).
- Reference implementation (KC-1).
- Hardened services and infra automation (KC-2).
- Marketplace foundations (KC-3).
- Production launch with commercial operations (KC-4).

## Dependencies and Risks

- Avoid coupling to AI or external connector implementations early; keep contracts stable.
- External infra dependencies should be isolated behind clear adapters.

## Milestones

- M1: Architecture approval (KC-0 complete).
- M2: First end-to-end pack build (KC-1 complete).
- M3: Production-like staging environment (KC-2 complete).
- M4: Marketplace MVP (KC-3 complete).
- M5: Commercial launch (KC-4 complete).

## Immediate Next Milestones (additions)

- **Contract tests & spec validation (early KC-1):** implement contract test suites for manifest schema, event payload shapes, and pack verification to prevent divergent implementations.
- **Key management prototype (KC-2):** design and prototype registry-backed key discovery and signing workflows.
- **Monetization schema (KC-2/KC-3):** finalize minimal monetization and entitlement fields and wire billing hooks for KC-3.


---
_Roadmap aligns technical milestones to business objectives and minimizes scope creep by deferring AI, connectors, and runtime services to later phases._
