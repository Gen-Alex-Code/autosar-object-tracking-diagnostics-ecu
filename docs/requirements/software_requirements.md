# Software Requirements

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## REQ-SW-001 — ObjectCandidate Data Type

**Status:** Draft  
**Owner:** Software Architect / Application SWC Developer  
**Verification:** Review + data model unit test  

The software shall define an `ObjectCandidate_T` data type containing at least:

```text
object_id
distance_cm
relative_velocity_cm_s
confidence_percent
age_cycles
quality
timestamp_ms
```

The `object_id` shall be treated as an upstream-provided runtime identifier. The ECU shall not generate globally persistent object IDs.

---

## REQ-SW-002 — ObjectCandidateList Data Type

**Status:** Draft  
**Owner:** Software Architect  
**Verification:** Boundary test  

The software shall define a bounded `ObjectCandidateList_T` data type.

The data type shall include:

| Field | Purpose |
|---|---|
| `candidates` | Fixed-size storage for candidate entries. |
| `candidate_count` | Number of valid entries in the list. Only indices `0` to `candidate_count - 1` shall be processed. |
| `source_status` | Health/freshness status of the whole candidate source or list. |
| `timestamp_ms` | Timestamp used to reason about list freshness and timeout behavior. |

The list shall not require dynamic memory allocation during `ObjectTracker_MainCycle()`.

---

## REQ-SW-003 — Sensor Quality Type

**Status:** Draft  
**Owner:** Software Architect  
**Verification:** Candidate quality tests  

The software shall define `SensorQuality_T` for individual candidate quality.

Allowed values:

```text
Good
Degraded
Invalid
Timeout
```

This status belongs to one candidate.

---

## REQ-SW-004 — Sensor Source Status Type

**Status:** Draft  
**Owner:** Software Architect / Diagnostics Developer  
**Verification:** Source-status tests  

The software shall define `SensorSourceStatus_T` for the whole candidate list/source.

Allowed values:

```text
Good
Degraded
Invalid
Timeout
```

Purpose:

```text
SensorQuality_T:
    quality of one candidate

SensorSourceStatus_T:
    validity or freshness of the complete candidate list/source
```

Expected definition location:

```text
include/generated/rte/Rte_Types.hpp
```

Expected users:

```text
SensorInputSWC
CandidateListValidator
ObjectTrackerSWC
DEM-style diagnostic reporting
DCM-style diagnostic snapshot
```

If `source_status` is `Invalid` or `Timeout`, candidate processing shall be skipped and diagnostic behavior shall follow `diagnostic_requirements.md`.

---

## REQ-SW-005 — Candidate Decision Type

**Status:** Draft  
**Owner:** Software Architect / Application SWC Developer  
**Verification:** Candidate decision unit tests  

The software shall define `CandidateDecision_T` with:

```text
Accepted
Rejected
Ignored
```

The decision shall be assigned per candidate, per cycle.

---

## REQ-SW-006 — Tracking State Type

**Status:** Draft  
**Owner:** Software Architect  
**Verification:** Lifecycle state tests  

The software shall define `TrackingState_T` with:

```text
Inactive
Tentative
Active
Coasting
Lost
```

The lifecycle transition rules shall be defined in `tracking_lifecycle_requirements.md`.

---

## REQ-SW-007 — RTE-Based Data Access

**Status:** Draft  
**Owner:** Application SWC Developer / RTE Developer  
**Verification:** RTE integration test  

`ObjectTrackerSWC` shall access input and output data through RTE-style functions.

Allowed pattern:

```cpp
Rte_Read_ObjectCandidateList(&candidate_list);
Rte_Write_TrackedObjectStatus(&tracking_status);
Rte_Write_ObjectAlertOutput(alert_state);
```

Not allowed inside SWC:

```cpp
Gpio_WritePin(...);
Can_Send(...);
Eeprom_Write(...);
```

---

## REQ-SW-008 — Runtime State Persistence

**Status:** Draft  
**Owner:** Software Architect / Diagnostics Developer  
**Verification:** Review + NvM-style unit test  

The following values shall be RAM-only:

```text
current tracking state
primary object ID
last confidence
missed cycle count
candidate decisions
```

The following values may be persisted using NvM-style storage:

```text
false-positive counter total
accepted-object counter total
selected diagnostic event history
```

---

## REQ-NF-001 — Programming Language and Code Quality

**Status:** Draft  
**Owner:** Software Architect / Developer  
**Verification:** Build + static-analysis review  

The software shall be implemented in C++17.

The project shall use language and quality practices that provide real benefit for a desktop-executable AUTOSAR-inspired simulation:

```text
clang-format:
    planned and already supported through .clang-format

clang-tidy:
    recommended for host-side static-analysis checks using practical profiles
    such as bugprone, readability, performance, modernize, and selected C++ Core Guidelines checks

AUTOSAR/MISRA-style review:
    planned as a documented review checklist and optional clang-tidy-supported subset,
    without claiming full certified AUTOSAR or MISRA compliance

RAII:
    allowed and preferred for resource ownership in infrastructure/test code,
    but runtime SWC logic shall remain deterministic and simple

sanitizers:
    recommended for host debug builds, especially AddressSanitizer and UndefinedBehaviorSanitizer

dynamic allocation:
    prohibited inside the 10 ms main tracking cycle
```

---

## REQ-NF-002 — Build System and Dependencies

**Status:** Draft  
**Owner:** Build / Integration Developer  
**Verification:** CMake build test  

The project shall use CMake.

For the MVP, dependency handling shall remain simple.

Recommended approach:

```text
CMake + FetchContent for GoogleTest
```

Conan, CPM, or other package managers may be considered later if dependency complexity increases, but they are not required for the MVP.
