# SWC Traceability

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 3 — SWC Architecture  
**Status:** Draft v3

---

## 1. Purpose

This document maps Phase 2 requirements to the Phase 3 SWC architecture.

---

## 2. Requirement-to-SWC Mapping

| Requirement | SWC / architecture element | Notes |
|---|---|---|
| `REQ-SYS-001` | `SensorInputSWC`, `ObjectCandidateList_I` | Candidate list reception/production. |
| `REQ-SYS-002` | `ObjectTrackerSWC`, `CandidateValidator` | Candidate validation. |
| `REQ-SYS-003` | `ObjectTrackerSWC`, `TrackingStateMachine` | Active object lifecycle. |
| `REQ-SYS-004` | `ObjectTrackerSWC`, `DiagnosticEvent_I`, `DiagnosticCounter_I` | Rejection tracking and diagnostics. |
| `REQ-SYS-005` | `ObjectAlertOutput_I` | Application alert output. |
| `REQ-SYS-006` | `PrimaryObjectSelector` | Multiple accepted candidates. |
| `REQ-SYS-007` | `TrackingStateMachine` | Reappearance handling. |
| `REQ-SYS-008` | `ObjectTracker_DiagnosticRead`, DCM-style path | Functional diagnostic visibility. |
| `REQ-SW-001` | `ObjectCandidate_T` | Candidate data type. |
| `REQ-SW-002` | `ObjectCandidateList_T` | Bounded candidate list. |
| `REQ-SW-004` | `SensorSourceStatus_T` | Source/list status. |
| `REQ-SW-007` | RTE contract preview | RTE-based data access. |
| `REQ-TRACK-*` | `TrackingStateMachine` inside `ObjectTrackerSWC` | Lifecycle transitions. |
| `REQ-VAL-*` | `CandidateValidator`, `PrimaryObjectSelector` inside `ObjectTrackerSWC` | Validation and decision logic. |
| `REQ-DIAG-*` | `ObjectTracker_DiagnosticRead`, future DEM/DCM/NvM | Diagnostic reads/events/counters. |
| `REQ-TIME-*` | `SensorInput_MainCycle`, `ObjectTracker_MainCycle`, scheduler | Timing and runnable order. |

---

## 3. SWC-to-Future-Code Mapping

| SWC | Main docs | Future code |
|---|---|---|
| `SensorInputSWC` | `swc_architecture.md`, `swc_interfaces.md`, `runnable_model.md` | `include/application/SensorInputSWC.hpp`, `src/application/SensorInputSWC.cpp` |
| `ObjectTrackerSWC` | `swc_architecture.md`, `data_type_model.md`, `runnable_model.md` | `include/application/ObjectTrackerSWC.hpp`, `src/application/ObjectTrackerSWC.cpp` |

---

## 4. Internal Modules Inside ObjectTrackerSWC

These are internal classes/modules, not separate SWCs:

```text
CandidateValidator
PrimaryObjectSelector
TrackingStateMachine
DiagnosticSnapshotBuilder
```

---

## 5. Code Traceability Example

```cpp
// REQ-SYS-002
// REQ-VAL-009
CandidateDecision_T CandidateValidator::evaluate(const ObjectCandidate_T& candidate)
{
    // implementation
}
```

---

## 6. Test Traceability Example

```cpp
// Verifies: REQ-TRACK-003
TEST(TrackingLifecycleTest, TentativeBecomesActiveAfterTwoDetectionsWithinThreeCycles)
{
    // test
}
```
