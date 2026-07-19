# RuleMark Terminology

Status: Frozen vocabulary for public and machine interfaces

| Concept | Fixed term | Meaning |
|---|---|---|
| Institutional system | Canonical Public Record Infrastructure | RuleMark's public identity, preservation, and resolution system |
| Current standards index | Canonical Standards Registry | The registered set and current status of RuleMark standards |
| One registered version | Canonical Record | The identity and metadata assigned to one frozen version |
| Original frozen file | Normative Artifact | The PDF bytes to which the canonical record refers |
| Historical evidence | Canonical Archive | Previous versions, status transitions, hashes, signatures, and distribution evidence |
| Publisher | RuleMark | The institution that assigns identities and publishes canonical records |
| Machine representation | Machine Record | Structured JSON derived from a canonical record |

## Standard statuses

- `FROZEN`: registered and immutable at the stated version.
- `SUPERSEDED`: preserved, but a later version is designated as its replacement.
- `WITHDRAWN`: preserved for history, but no longer designated for current use.
- `DRAFT`: an unpublished working state. Drafts must not appear in the Canonical Archive or Canonical Standards Registry.

`DEPRECATED` is reserved for software interfaces. `REVOKED` is reserved for credentials or authorizations. Neither term is a standard status.

## Identity rule

`RM-S-*` is the only canonical standard identifier namespace. Earlier identifiers such as `DAO-001` are legacy redirect aliases and must not be emitted as independent canonical identities.
