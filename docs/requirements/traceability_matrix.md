# Traceability Matrix

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## 1. Traceability Model

The project uses repository-native traceability:

```text
requirement ID
→ functional concept section
→ requirement document
→ future design element
→ future code element
→ future test evidence
→ research/supporting reference
```

Code traceability convention:

```cpp
// REQ-TRACK-003
```

Test traceability convention:

```cpp
// Verifies: REQ-TRACK-003
```

Research traceability convention:

```text
Related research: REF-002
```

---

## 2. Documentation Traceability

Important supporting documents shall be linked from the requirement metadata or the traceability matrix.

Allowed trace targets:

```text
functional concept section
requirements document
architecture diagram
state-machine diagram
research reference
source file
test file
review record
```

This mechanism imitates a lightweight DOORS-style workflow while remaining simple Markdown.

---

## 3. Phase 2 Traceability Table

| Requirement | Source document | Future design/code element | Future test evidence | Supporting doc/ref | Status |
|---|---|---|---|---|---|
| `REQ-SYS-001` | `system_requirements.md` | `ObjectCandidateList_T`, `Rte_Read_ObjectCandidateList()` | Candidate-list integration test | `functional_concept.md`, `REF-001` | Draft |
| `REQ-SYS-002` | `system_requirements.md` | `CandidateValidator`, `ObjectTracker_MainCycle()` | Candidate validation unit tests | `validation_policy.md`, `REF-001` | Draft |
| `REQ-SYS-003` | `system_requirements.md` | `TrackingStateMachine`, `TrackedObjectStatus_T` | Lifecycle state tests | `tracking_lifecycle_requirements.md`, `REF-002` | Draft |
| `REQ-SYS-004` | `system_requirements.md` | `RejectionReason_T`, counters, DEM calls | Counter and DEM tests | `diagnostic_requirements.md` | Draft |
| `REQ-SYS-005` | `system_requirements.md` | `ObjectAlertOutput_T`, RTE write path | Alert output tests | `functional_concept.md` | Draft |
| `REQ-SYS-006` | `system_requirements.md` | `PrimaryObjectSelector` | Multi-candidate selection tests | `validation_policy.md` | Draft |
| `REQ-SYS-007` | `system_requirements.md` | `TrackingStateMachine` | Reappearance tests | `tracking_lifecycle_requirements.md` | Draft |
| `REQ-SW-001` | `software_requirements.md` | `ObjectCandidate_T` | Data model tests | — | Draft |
| `REQ-SW-002` | `software_requirements.md` | `ObjectCandidateList_T` | Boundary tests | `validation_policy.md` | Draft |
| `REQ-SW-004` | `software_requirements.md` | `SensorSourceStatus_T` | Source-status tests | `diagnostic_requirements.md` | Draft |
| `REQ-SW-007` | `software_requirements.md` | `Rte_ObjectTrackerSWC.hpp`, `Rte.cpp` | RTE integration tests | `REF-005` | Draft |
| `REQ-NF-001` | `software_requirements.md` | `.clang-format`, clang-tidy config, sanitizer build option | Static-analysis/build review | Future quality checklist | Draft |
| `REQ-TRACK-003` | `tracking_lifecycle_requirements.md` | `confirmCandidate()` | M-out-of-N test | `REF-002` | Draft |
| `REQ-TRACK-008` | `tracking_lifecycle_requirements.md` | `updateCoasting()` | Deletion threshold test | `REF-002` | Draft |
| `REQ-VAL-004` | `validation_policy.md` | `CandidateListValidator` | Source-status validation tests | `diagnostic_requirements.md` | Draft |
| `REQ-VAL-012` | `validation_policy.md` | `PrimaryObjectSelector` | Selection tests | `REF-001` | Draft |
| `REQ-DIAG-002` | `diagnostic_requirements.md` | `Dcm_ReadTrackingStatus()` | DCM status read test | `functional_concept.md` | Draft |
| `REQ-DIAG-004` | `diagnostic_requirements.md` | `Dcm_ReadLastObjectConfidence()` | Confidence read test | `functional_concept.md` | Draft |
| `REQ-DIAG-008` | `diagnostic_requirements.md` | `Dcm_ReadTrackingSnapshot()` | Snapshot read test | `functional_concept.md` | Draft |
| `REQ-TIME-001` | `timing_requirements.md` | `Task_10ms`, `SchM_Rte.cpp` | Scheduler test | `REF-006` | Draft |
| `REQ-TIME-004` | `timing_requirements.md` | `ObjectTracker_MainCycle()` | Timing budget test | `REQ-TIME-002`, `REQ-TIME-003` | Draft |
| `REQ-TIME-006` | `timing_requirements.md` | Code review | Code review checklist | Future quality checklist | Draft |

---

## 4. Traceability Open Items

The following traces will become concrete after implementation starts:

```text
source file names
function names
unit test names
integration test names
static-analysis evidence
timing measurement evidence
review records
```
