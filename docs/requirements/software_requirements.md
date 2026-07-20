# Software Requirements

## Application Layer

| ID | Requirement | Design Element | Verification |
|---|---|---|---|
| REQ-SW-APP-001 | `SensorInputSWC` shall provide `ObjectCandidateListOut`. | `SensorInputSWC` | Integration test |
| REQ-SW-APP-002 | `ObjectTrackerSWC` shall read `ObjectCandidateListIn` through an RTE-style accessor. | `ObjectTrackerSWC`, RTE API | Unit/integration test |
| REQ-SW-APP-003 | `ObjectTrackerSWC` shall evaluate candidates using deterministic gating logic. | `ObjectTracker_MainCycle()` | Unit tests |
| REQ-SW-APP-004 | `ObjectTrackerSWC` shall update tracking state when a valid object is accepted. | `ActiveObjectState` | Unit tests |
| REQ-SW-APP-005 | `ObjectTrackerSWC` shall update false-positive counters when invalid candidates are rejected. | `FalsePositiveCounter` | Unit tests |
| REQ-SW-APP-006 | `ObjectTrackerSWC` shall provide `ObjectAlertOutput`. | RTE output port | Integration test |

## RTE Layer

| ID | Requirement | Design Element | Verification |
|---|---|---|---|
| REQ-SW-RTE-001 | The simulated generated RTE shall provide read access for `ObjectCandidateListIn`. | `Rte_ObjectTrackerSWC.hpp`, `Rte.cpp` | RTE unit test |
| REQ-SW-RTE-002 | The simulated generated RTE shall provide write access for `TrackedObjectStatusOut`. | `Rte_ObjectTrackerSWC.hpp`, `Rte.cpp` | RTE unit test |
| REQ-SW-RTE-003 | The simulated generated RTE shall route diagnostic event calls to the DEM concept. | `Rte.cpp`, `Dem.cpp` | Integration test |
| REQ-SW-RTE-004 | The simulated scheduler glue shall activate `ObjectTracker_MainCycle()` according to the 10 ms timing event. | `SchM_Rte.cpp` | Scheduler integration test |

## BSW Layer

| ID | Requirement | Design Element | Verification |
|---|---|---|---|
| REQ-SW-BSW-001 | The DEM concept shall store diagnostic event status for invalid or timeout sensor quality. | `Dem.cpp` | DEM unit test |
| REQ-SW-BSW-002 | The DCM concept shall expose tracking status and counters through simplified diagnostic reads. | `Dcm.cpp` | DCM unit test |
| REQ-SW-BSW-003 | The NvM concept shall preserve selected diagnostic counters. | `NvM.cpp` | NvM unit test |
| REQ-SW-BSW-004 | The OS concept shall provide a 10 ms task activation path. | `Os.cpp`, `SchM_Rte.cpp` | Scheduler integration test |
