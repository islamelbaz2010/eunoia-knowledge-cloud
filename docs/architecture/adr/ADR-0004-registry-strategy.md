# ADR-0004 — Registry Strategy: File-backed Proof-of-Concept with Migration Adapter

**Status:** Accepted

**Context:**
EKC requires authoritative registries for Sources, Datasets, Packs, Templates, Schemas, Taxonomy, and Versions. Registries must support: (1) versioned metadata, (2) discovery and filtering, (3) multi-tenancy for marketplace and enterprise, (4) scale to millions of artifacts. Choosing the wrong backend early causes expensive later refactoring.

**Problem:**
EKC is in KC-0 (architecture sprint) and KC-1 will be a proof-of-concept implementation. Committing to a distributed database or choosing a specific vendor (e.g., Postgres, Spanner, Cosmos) early creates risk of lock-in or architectural mismatch. However, we need working registries to validate the architecture. A flexible strategy that enables safe evolution is required.

**Decision:**
Implement registry as a layered abstraction:
- **Storage adapter interface:** well-defined interface for registry persistence (get, register, list, deprecate).
- **KC-1 proof-of-concept:** file-backed implementation (JSON/YAML files on disk or object storage).
- **Migration path:** adapter pattern enables swapping storage backends for KC-2+ without changing registry APIs.
- **Multi-tenancy:** design registries with namespace/tenant partitioning from the start, enabling multi-tenant support in KC-3.

**Rationale:**
- **Flexibility:** adapter pattern prevents vendor lock-in and enables choosing production datastore based on scale data from KC-1.
- **Time-to-value:** file-backed implementation is quick to build, enabling architecture validation without infrastructure setup.
- **Scalability:** designing for partitioning/sharding now prevents architectural surgery later.
- **Cost management:** proof-of-concept doesn't require expensive managed database; production choice can be optimized later.

**Alternatives Considered:**
1. **Monolithic RDBMS (Postgres) from day 1:** Fast to develop, but difficult to migrate if scale or multi-tenancy needs change. Rejected (tight coupling).
2. **Document DB (Firestore, MongoDB) from day 1:** Good for semi-structured data, but vendor lock-in. Rejected (early commitment).
3. **Custom file-backed system:** Builds registry logic from first principles, teaching. **Chosen** for KC-1.
4. **Cloud-agnostic datastore (TiDB, Cockroach):** Good but adds infrastructure complexity for PoC. Deferred to KC-2.

**Consequences:**
- ✅ Low upfront infrastructure complexity.
- ✅ Enables architecture validation before committing to storage backend.
- ✅ Adapter pattern protects against lock-in.
- ✅ Multi-tenant design now prevents later rework.
- ⚠️ File-backed registries won't scale to millions of artifacts (load tests will identify limits).
- ⚠️ Requires careful adapter design to avoid leaking storage assumptions into APIs.
- ⚠️ Consistency model may differ between file-backed and production datastore (must be understood).

**Implementation Notes:**
- Registry interfaces: `register(entity)`, `get(id, version)`, `list(filters, pageToken)`, `deprecate(id, version)`.
- File-backed storage: store entities as JSON in canonical paths: `registries/{type}/{tenantId}/{id}/{version}.json`.
- Indexes: maintain simple secondary indexes (by packId, version, taxonomy terms) as separate index files.
- Consistency: file-backed registries provide strong read-after-write semantics; production datastore may have different guarantees (to be specified in KC-2).
- Migration testing: create contract tests that work against both file-backed and future storage adapters.
- Pagination: implement cursor-based pagination from the start to support large result sets.
- Partitioning: registries are logically partitioned by `{registryType}/{tenantId}`, enabling future sharding.

**Related Decisions:**
- ADR-0001 (Architecture Freeze): registry strategy is frozen but implementation details can evolve via ADRs in KC-2.
- ADR-0002 (Pack Contract): pack registry must store and publish manifests and signer metadata.
- Contract: `docs/specifications/registry.schema.json`.

**Approval:**
- Architecture Board: Approved 2026-07-03
- Document: ARCHITECTURE_VERSION.md, docs/architecture/05-registry-design.md

**Supersedes:**
None (first specification of registry strategy).

---

## Migration Path (informational)

If load tests in KC-1 show file-backed limits, KC-2 will evaluate distributed datastores:
- **Postgres + partitioning:** good for structured metadata, familiar ops.
- **Cloud SQL (Spanner, BigTable):** managed, global consistency, high cost.
- **MongoDB / DocumentDB:** semi-structured, easier schema evolution.
- **Custom-built distributed system:** full control, highest risk.

Decision will be made based on KC-1 scale data and informed by an ADR in KC-2.
