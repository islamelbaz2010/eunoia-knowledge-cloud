# Eunoia AI OS — Knowledge Pack Consumption Contract

This document defines the exact contract by which Eunoia AI OS (the consumer) must consume Knowledge Packs produced by EKC. It is an interface contract only — no implementation details.

## Objectives

- Ensure deterministic, versioned consumption semantics.
- Define manifest and content access expectations.
- Define compatibility, error semantics, and expected integration behavior.

## Discovery & Retrieval

- Eunoia AI OS discovers packs via the Pack Registry. The consumer resolves a `packRef` (packId + version) to a download URL or storage pointer.
- The consumer must verify `manifest.json` and `checksums/` prior to trusting content.

## Manifest Contract

- The consumer must read `manifest.json` from `/manifests/manifest.json` and validate the following fields: `packId`, `version`, `components[]`, `compatibility`.
- Consumers must validate `manifest.sig` using publisher public keys (key discovery is out-of-scope for KC-0; keys will be provided via registry in later phases).

## Integrity and Verification

- Consumers must validate checksums in `checksums/checksums.sha256` and cross-check file paths listed in `components`.

## Compatibility Rules

- Consumers must check `compatibility.minimum` and `compatibility.maximum` in the manifest; if the consumer version is outside the range it must refuse to load and surface a clear error code `ERR_INCOMPATIBLE_CONTRACT`.

## Consumption Semantics

- Packs are read-only; consumers may cache locally but must not mutate pack blobs.
- Consumers should apply a deterministic unpacking procedure and preserve provenance metadata for any downstream operations.

## Error Codes & Recovery

- `ERR_MISSING_MANIFEST` — manifest not found; abort.
- `ERR_CHECKSUM_MISMATCH` — integrity failure; abort and report incident.
- `ERR_SIGNATURE_INVALID` — signature verification failed; abort.
- `ERR_INCOMPATIBLE_CONTRACT` — contract mismatch; avoid applying pack.

Recovery: Consumers should log incidents and fall back to the last known-good pack or operate in degraded mode according to policy.

## Observability

- Consumers must report pack `packId`, `version`, `packChecksum`, `consumeTimestamp`, and `validationStatus` back to the registry or telemetry stream.

## Security

- Consumers must run pack scanners in a sandboxed environment and not execute arbitrary code included in packs.

## Extension Points

- Consumers may support manifest extensions but must ignore unknown namespaces by default.

---
_This contract ensures Eunoia AI OS can safely and deterministically consume Knowledge Packs produced by EKC._

## Key Discovery & Trust Model

- Consumers MUST resolve the publisher public key via the Pack Registry `signerId` metadata before trusting `manifest.sig`. Registry-stored key metadata is the recommended initial approach; later phases may integrate HSM or global PKI.

## Extended Error Codes

- `ERR_KEY_NOT_FOUND` — signer metadata missing or key not resolvable; abort.
- `ERR_SIGNATURE_EXPIRED` — signature timestamp outside trusted window.
- `ERR_RATE_LIMITED` — consumer exceeded allowed retrieval rate; backoff and retry per policy.

## Operational Notes for Consumers

- Consumers should implement local caches for keys and manifests with TTLs and validate cache freshness against the Pack Registry.
- Consumers must follow a deterministic, auditable unpacking process and persist provenance metadata for downstream audits.

