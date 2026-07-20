# Traceability Matrix

| Requirement ID | Design Element | Source File | Test Case | Status |
|---|---|---|---|---|
| REQ-F-001 | Candidate-list input | `Rte_Types.hpp`, `Rte.cpp` | `test_ObjectTrackingEcuFlow.cpp` | Draft |
| REQ-F-002 | Candidate validation | `ObjectTrackerSWC.cpp` | `test_ObjectTrackerSWC.cpp` | Draft |
| REQ-F-003 | Confidence threshold | `ObjectTrackerSWC.cpp` | `test_ObjectTrackerSWC.cpp` | Draft |
| REQ-F-004 | False-positive counter | `ObjectTrackerSWC.cpp`, `NvM.cpp` | `test_ObjectTrackerSWC.cpp` | Draft |
| REQ-F-005 | Active object state | `ObjectTrackerSWC.cpp` | `test_ObjectTrackerSWC.cpp` | Draft |
| REQ-F-006 | Tracking status DID | `Dcm.cpp` | `test_Dcm.cpp` | Draft |
| REQ-F-007 | False-positive counter DID | `Dcm.cpp`, `NvM.cpp` | `test_Dcm.cpp`, `test_NvM.cpp` | Draft |
| REQ-F-008 | DEM event reporting | `ObjectTrackerSWC.cpp`, `Dem.cpp`, `Rte.cpp` | `test_Dem.cpp` | Draft |
| REQ-F-009 | Counter persistence | `NvM.cpp` | `test_NvM.cpp` | Draft |
| REQ-F-010 | 10 ms runnable activation | `Os.cpp`, `SchM_Rte.cpp` | `test_Scheduler.cpp` | Draft |
| REQ-F-011 | Object alert output | `ObjectTrackerSWC.cpp`, `Rte.cpp` | `test_ObjectTrackerSWC.cpp` | Draft |
| REQ-NF-001 | CMake build | `CMakeLists.txt` | Build command | Draft |
| REQ-NF-002 | C++17 | `CMakeLists.txt` | Build command | Draft |
| REQ-NF-TIME-001 | 10 ms completion | `ObjectTrackerSWC.cpp` | timing/profiling test | Draft |
| REQ-NF-TIME-002 | bounded candidate list | `Rte_Types.hpp` | unit/integration tests | Draft |
| REQ-NF-TIME-003 | O(n) complexity | `ObjectTrackerSWC.cpp` | code review | Draft |
