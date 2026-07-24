# Research Index

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## 1. Purpose

This document records supporting references and engineering concepts used to justify selected requirements.

The references are not copied into the requirements. They are linked through the traceability matrix to show why certain decisions exist.

---

## 2. Reference IDs

| ID | Topic | Use in project |
|---|---|---|
| `REF-001` | Simple online real-time tracking concepts | Supports simple MVP approach, detection-quality emphasis, and primary-object selection. |
| `REF-002` | Track confirmation/deletion concepts | Supports Tentative, Active, Coasting, and Lost lifecycle behavior. |
| `REF-003` | Bounded tracker resources | Supports `kMaxObjectCandidates` and bounded runtime. |
| `REF-004` | Candidate gating / validation | Supports confidence, distance, and quality-based validation. |
| `REF-005` | AUTOSAR Classic layering and RTE/BSW separation | Supports SWC/RTE/BSW responsibility boundaries. |
| `REF-006` | AUTOSAR-style timing events and cyclic runnable activation | Supports 10 ms scheduler/runnable model. |
| `REF-007` | Embedded timing and deterministic execution practices | Supports no dynamic allocation in the main cycle. |

---

## 3. Notes

This project uses references as engineering support, not as claims of production compliance.

The reference index should remain lightweight and should only include sources that directly support design or requirement decisions.
