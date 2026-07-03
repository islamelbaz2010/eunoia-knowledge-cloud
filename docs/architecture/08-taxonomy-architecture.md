# Taxonomy Architecture

The taxonomy provides canonical identifiers and hierarchical relationships for countries, industries, languages, knowledge types, document types, categories, and tags. It drives classification, discovery, and policy decisions.

## Scope

- Countries (ISO-based with regional groupings)
- Industries (vertical taxonomy tuned for enterprise use)
- Languages (BCP-47 codes)
- Knowledge Types (procedural, regulatory, reference, product, training)
- Document Types (article, spec, checklist, template, policy)
- Categories & Tags (user-defined controlled vocabularies)

## Design Principles

- Canonical IDs: every node has a stable identifier (e.g., `country:EG`, `industry:hotels`).
- Hierarchies and facets: support multi-parent relationships and faceted tagging.
- Versioned: taxonomy changes are versioned and discoverable.
- Localization: human-visible labels and synonyms per language.

## Data Model

- `termId`: string
- `labels`: { lang: label }
- `parents`: [termId]
- `metadata`: free-form structured payload
- `mappings`: external identifiers (ISO, NAICS, etc.)

## Governance

- Ownership: taxonomy owners manage term additions and mappings.
- Change Process: updates go through a review workflow and are versioned.

## Integration Points

- Registry entries reference taxonomy terms by `termId`.
- Validator enforces mandatory taxonomy fields for jurisdiction-sensitive packs.

## Use Cases

- Region-filtered pack discovery.
- Industry-specific validation rules.
- Language fallback and localization selection.

## Extensibility

- Support for derived taxonomies (client-specific) via mapping layers.
- Tagging overlays that allow ephemeral tags for curation without altering canonical taxonomy.

---
_Taxonomy is the lingua franca for classification across EKC and downstream consumers._
