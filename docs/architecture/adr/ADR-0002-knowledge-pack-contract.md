# ADR-0002 — Knowledge Pack Format & Manifest Contract

**Status:** Accepted

**Context:**
EKC produces Knowledge Packs for consumption by Eunoia AI OS and downstream systems. The pack format must be: (1) self-verifiable without external trust (checksums, signatures), (2) extensible for marketplace monetization, (3) auditable (manifest captures origin, versioning, provenance), and (4) compatible across consumer versions.

**Problem:**
Without a canonical pack format and manifest contract, implementations may produce incompatible artifacts, consumers may struggle to verify integrity, and the marketplace cannot enforce licensing or entitlements. A clear specification is required.

**Decision:**
Define the Knowledge Pack as a directory structure with:
- `manifest.json`: core metadata (packId, version, components, provenance, compatibility, monetization).
- `manifest.sig`: detached signature (separate from manifest, enabling post-hoc verification).
- `checksums/checksums.sha256`: artifact checksums in canonical order.
- `content/`: knowledge artifacts (articles, schemas, assets).
- `metadata/`: provenance, taxonomy, license.
- `license/license.txt`: human-readable and machine-readable licensing.

**Rationale:**
- **Self-verification:** Detached signatures and checksums enable consumers to verify pack integrity without requiring pack modification.
- **Extensibility:** Manifest extensions allow marketplace and regional metadata without breaking consumers (who ignore unknown extensions).
- **Provenance:** Manifest records source, generation run, author, enabling audit trails.
- **Compatibility:** Manifest explicitly declares consumer contract versions (minimum/maximum), enabling graceful degradation.
- **Monetization:** Built-in monetization and licensing fields support future marketplace without schema redesign.

**Alternatives Considered:**
1. **Monolithic binary blob:** Difficult to audit, verify, and inspect. Rejected.
2. **Signed entire pack (tar + signature):** Prevents independent component verification. Rejected.
3. **Manifest embedded in pack:** Manifest modification breaks signature. Rejected.
4. **Detached manifest + signatures** (chosen): Enables independent verification and future monetization.

**Consequences:**
- ✅ Consumers can verify integrity without trusting pack registry.
- ✅ Detached signature enables future key rotation without repacking.
- ✅ Manifest extensions support marketplace features without schema breaks.
- ✅ Clear provenance trail for auditing.
- ⚠️ Requires consumers to verify checksums (operational burden, but tooling provided).
- ⚠️ Manifest extensions could create fragmentation if not carefully governed.

**Implementation Notes:**
- Pack Registry stores and publishes pack metadata and checksums.
- Publisher generates manifest and signature using configured signing algorithm (algorithm agnostic; RC2/ECDSA/etc. in implementation).
- Consumers must verify manifest.sig using public keys from Pack Registry (see ADR-0004 on registry strategy).
- Manifest.extensions namespace is governed: only known extensions are processed, unknown ones ignored.
- Future marketplace features (pricing, entitlements) use monetization and license fields.

**Related Decisions:**
- ADR-0004 (Registry Strategy): defines how manifest versions and signer keys are stored and discovered.
- Contract: `docs/specifications/manifest.schema.json`, `docs/specifications/knowledge-pack.schema.json`.

**Approval:**
- Architecture Board: Approved 2026-07-03
- Document: ARCHITECTURE_VERSION.md, docs/architecture/07-knowledge-pack-specification.md

**Supersedes:**
None (first specification of pack format).
