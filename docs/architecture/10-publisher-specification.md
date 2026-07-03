# Publisher Specification

The Publisher is responsible for final pack integrity, signing, channel promotion, and distribution governance.

## Responsibilities

- Verify pack-level checksums and component checksums.
- Generate detached signatures for manifests and optionally for the whole pack.
- Enforce release policies and channel promotions (draft → staged → stable).
- Produce publish receipts and register published packs in the Pack Registry.

## Versioning & Channels

- Channels: `draft`, `staged`, `stable`.
- Promotion rules: automated promotions only after `ValidationPassed` and policy checks; manual promotion supported for overrides.

## Integrity & Signing

- Signing algorithm: canonical, algorithm-agnostic specification (e.g., support for RSA/ECDSA modern algorithms in implementation phase).
- Signature metadata includes `signerId`, `keyId`, and `signatureAlgorithm`.

## Distribution

- Packs are made available via Pack Registry references. Distribution targets (CDNs, object stores) are implementation details for KC-1+.

## Publish Receipt

- A published pack must produce a `PublishReceipt` containing: `packId`, `version`, `channel`, `publishedAt`, `signatureRef`, `checksums`, and `publishActor`.

## Rollback & Deprecation

- Deprecation path: `deprecate(packRef, reason)` produces `PackDeprecated` event and marks the pack as deprecated in the registry.
- Rollbacks are new versions that supersede earlier versions; immutable history is preserved.

## Security

- Signing keys are stored in a secure key store with rotation policies and ephemeral signing tokens for automation.

## Errors

- Integrity mismatch: abort and raise alert to governance.
- Signing failure: abort and require manual remediation.

---
_Publisher ensures that only validated and signed Knowledge Packs reach consumers and that distribution is auditable and reversible._

## Key Discovery & Signing Governance

- The Pack Registry will publish `signerId` metadata (keyId, algorithm, validity) enabling consumers to resolve and verify `manifest.sig`.
- Signing keys must be managed with rotation policies; ephemeral signing tokens for automation are allowed but require audit trails.

## Entitlements & Distribution Controls

- The Publisher enforces entitlements and distribution constraints declared in `manifest.monetization` and `manifest.license` before promoting a pack to `staged` or `stable` channels.
- Integration points with billing systems should be planned in KC-3 for paid packs.
