# RuleMark System Specification

## Naming · Directory · Templates · Control Plane

**Document ID:** RM-S-SPEC-001
**Version:** v1.0-F
**Status:** FROZEN · ACTIVE · ENFORCED
**Scope:** V1 (Human) · V2 (Machine) · V3 (Capital) · V4 (AI)
**Authority:** Supreme — All subsystems MUST comply

> This document constitutes the **Measurement Convention** of RuleMark.
> Any modification equals a **constitutional amendment**.

---

## Part I. Version Layering (Canonical Definition)

| Layer  | Audience          | Function                | Enforcement      |
| ------ | ----------------- | ----------------------- | ---------------- |
| **V1** | Human / Auditor   | Legal text, definitions | Git immutability |
| **V2** | System / Indexer  | Metadata, parsing       | Strict schema    |
| **V3** | Contract / Oracle | Financial parameters    | Oracle-ready     |
| **V4** | AI Agent          | Deterministic execution | Machine summary  |

**Principle**

* One **Canonical ID** governs all layers
* Versions evolve, identity never changes

---

## Part II. Naming Convention (Identifiers)

### 1. Canonical ID (Logical Entity · Eternal)

**Format**

```
[Domain]-[Type]-[Topic]-[###]
```

**Example**

```
RM-P-PEG-001
```

**Rules**

* Used in contracts, citations, external references
* NEVER contains version or status
* NEVER changes once issued

---

### 2. Physical File Name (Storage Entity)

**Format**

```
[Canonical_ID].v[X.X]-[Status].[ext]
```

**Example**

```
RM-P-PEG-001.v1.0-F.md
```

---

### 3. Field Definitions

| Field  | Meaning       | Allowed Values                              |
| ------ | ------------- | ------------------------------------------- |
| Domain | System area   | RM / GOV / OPS / INF                        |
| Type   | Document type | P (Precedent), S (Standard), G (Governance) |
| Topic  | Subject       | 3–6 uppercase letters                       |
| ###    | Sequence      | 3 digits (001…)                             |
| Status | Legal state   | D / A / F                                   |

---

## Part III. Directory Topology (Physical Structure)

```
/rulemark-canonical-archive
│
├── /documents
│   ├── SYSTEM-SPEC-V1-V4.v1.0-F.md
│   └── ARCHITECTURE-V2.md
│
├── /precedents
│   ├── RM-P-PEG-001.v1.0-F.md
│   └── RM-P-PEG-001.v1.0-F.sig
│
├── /standards
│   ├── RM-S-RWA-001.v1.0-F.md
│   └── RM-S-RWA-001.v1.0-F.sig
│
├── /governance
│   └── CONSTITUTION-V2.md
│
├── /schemas
│   └── yaml-schema-v2.json
│
└── registry.json   ← auto-generated, immutable index
```

---

## Part IV. Authoritative Document Structure (The Mold)

### The Golden YAML Frontmatter

**MANDATORY for all Precedent / Standard files**

```yaml
---
# === V1 & V2: Identification ===
id: RM-S-TOPIC-000.v1.0-F
canonical_id: RM-S-TOPIC-000
title: "Insert Title Here"
type: Standard
status: Frozen
author: "RuleMark Architecture"
created: 2026-01-10

# === V2: File Integrity & Schema Control ===
file_integrity:
  hash: sha256:<hex>          # MUST match file content
  schema_version: "2.0"       # YAML schema version

dependencies:
  - RM-S-XXXX-000.v1.0-F      # Optional, explicit only

# === V3: Capital & Oracle Interface ===
# ONLY: number | boolean | enum string | null
oracle_data:
  min_value: 0
  is_active: true
  currency: "USDT"
  risk_level: 1

# === V4: AI Execution Logic ===
# Deterministic logic only
machine_summary: >
  IF condition_met IS true
  THEN execute_action allowed.
  FAIL_CONDITION: collateral < min_value.
  FALLBACK → HUMAN_REVIEW.
  CONFIDENCE ≥ 0.95.
---
```

**Hard Rules**

* Missing YAML → CI FAIL
* Natural language in `oracle_data` → FAIL
* Missing `machine_summary` → FAIL

---

## Part V. Signature File Specification (.sig)

**Filename**

```
[Physical_Filename].sig
```

**Content (JSON · Mandatory)**

```json
{
  "target_file": "RM-S-TOPIC-000.v1.0-F.md",
  "target_hash": "sha256:<hex>",
  "signer_id": "did:rulemark:founder",
  "signature_algorithm": "ed25519",
  "signature_value": "<hex_or_base64>",
  "timestamp": "2026-01-10T14:00:00Z"
}
```

**Rule**

* Without this exact structure, signature is non-authoritative

---

## Part VI. Authority & Automation (Control Plane)

### 6.1 Freeze Authority

| Status | Required Authority                                          |
| ------ | ----------------------------------------------------------- |
| **F**  | did:rulemark:founder (Phase I) or 5/7 Committee (DAO Phase) |
| **A**  | 3/5 Architecture Committee                                  |
| **D**  | Open contribution                                           |

---

### 6.2 Registry Auto-Generation (Mandatory)

**Trigger**

* Git pre-commit hook OR CI on merge

**Failure Conditions**

* Duplicate Canonical ID → CI FAIL
* Missing YAML / `.sig` → CI FAIL

---

### 6.3 `registry.json` Canonical Schema (FROZEN)

```json
{
  "metadata": {
    "generated_at": "2026-01-10T14:30:00Z",
    "total_documents": 42,
    "integrity_hash": "sha256:<hex_of_sorted_canonical_ids>"
  },
  "documents": [
    {
      "canonical_id": "RM-P-PEG-001",
      "latest_version": "v1.0-F",
      "status": "Frozen",
      "type": "Precedent",
      "file_path": "/precedents/RM-P-PEG-001.v1.0-F.md",
      "signature_path": "/precedents/RM-P-PEG-001.v1.0-F.sig",
      "last_updated": "2026-01-10T14:00:00Z"
    }
  ]
}
```

**Hard Rules**

* `registry.json` **only auto-generated**
* `integrity_hash` = SHA256(sorted canonical_id list)
* Any mismatch → system invalid

---

### 6.4 Immutable Laws

1. **Never Rename** — Frozen filenames are law
2. **Never Overwrite** — Changes require new version
3. **Fail Fast** — Non-compliance halts the system

---

## Final Declaration

This specification is **FROZEN**.
It defines how **facts are created, validated, frozen, and consumed**
by humans, machines, capital, and AI.

> From this point forward:
> **to change this document is to amend the constitution.**

---

