# System Requirements

## Functional Requirements

| ID | Requirement | Rationale | Verification |
|---|---|---|---|
| REQ-F-001 | The ECU shall receive a bounded list of object candidates every 10 ms. | Establishes the main cyclic input for object evaluation. | Integration test with simulated candidate list |
| REQ-F-002 | The ECU shall evaluate each object candidate using confidence, distance, age, and sensor quality. | Defines the MVP tracking decision logic. | Unit tests for candidate validation |
| REQ-F-003 | The ECU shall reject object candidates below the configured confidence threshold. | Filters weak detections before tracking. | Unit tests for threshold behavior |
| REQ-F-004 | The ECU shall increment a false-positive counter when an invalid object candidate is rejected. | Provides diagnostic and monitoring evidence. | Unit test for counter update |
| REQ-F-005 | The ECU shall maintain an active object state when a valid object candidate is accepted. | Represents simplified tracking state. | Unit test for accepted candidate |
| REQ-F-006 | The ECU shall expose the current tracking status through a simplified diagnostic read operation. | Supports diagnostics and observability. | DCM integration test |
| REQ-F-007 | The ECU shall expose the false-positive counter through a simplified diagnostic read operation. | Supports diagnostic inspection of rejection behavior. | DCM/NvM integration test |
| REQ-F-008 | The ECU shall report invalid or timeout sensor quality through a simplified DEM-style diagnostic event. | Models diagnostic event handling. | DEM service-call test |
| REQ-F-009 | The ECU shall preserve selected diagnostic counters using a simplified NvM-style persistence mechanism. | Models non-volatile diagnostic state. | NvM persistence test |
| REQ-F-010 | The ECU shall execute the main object-tracking runnable every 10 ms. | Establishes cyclic execution behavior. | Scheduler/RTE integration test |
| REQ-F-011 | The ECU shall provide an `ObjectAlertOutput` when a valid object is actively tracked or when a diagnostic-relevant object condition is detected. | Creates a future bridge to GPIO/MCU resource mapping. | Unit and integration test |

## Non-Functional Requirements

| ID | Requirement | Rationale | Verification |
|---|---|---|---|
| REQ-NF-001 | The project shall be buildable using CMake. | Enables portable desktop development. | Build execution |
| REQ-NF-002 | The project shall use C++17. | Establishes implementation baseline. | Compiler configuration |
| REQ-NF-003 | The project shall include unit tests for application logic. | Supports correctness evidence. | GoogleTest execution |
| REQ-NF-004 | The project shall include integration tests for RTE-style data flow. | Validates architecture beyond isolated functions. | Integration test execution |
| REQ-NF-005 | The project shall use a layered folder structure aligned with AUTOSAR-inspired responsibilities. | Improves architecture readability. | Repository review |
| REQ-NF-006 | The project shall clearly label generated/simulated generated code. | Avoids false production-AUTOSAR claims. | Code/documentation review |
| REQ-NF-007 | The project shall avoid claiming production AUTOSAR compliance. | Maintains technical honesty. | Documentation review |
| REQ-NF-TIME-001 | `ObjectTracker_MainCycle` shall complete within the configured 10 ms period under the supported MVP workload. | Ensures timing realism. | Timing test / execution profiling |
| REQ-NF-TIME-002 | The MVP workload shall process a bounded list of up to `N_MAX` object candidates per cycle. | Bounds runtime complexity. | Unit and integration tests |
| REQ-NF-TIME-003 | The baseline object evaluation algorithm shall have O(n) time complexity, where n is the number of candidates in the received candidate list. | Makes complexity explicit and reviewable. | Code review |
