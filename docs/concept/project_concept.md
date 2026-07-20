# Project Concept — autosar-object-tracking-diagnostics-ecu

## Project Name

`autosar-object-tracking-diagnostics-ecu`

## Display Title

AUTOSAR Classic Object Tracking Diagnostics ECU

## Project Classification

This repository is an AUTOSAR Classic-inspired educational ECU simulation implemented in C++.

It is not a production AUTOSAR stack and does not claim vendor AUTOSAR compliance.

## Purpose

The project provides a realistic, file-driven, artifact-driven explanation of how an AUTOSAR-inspired ECU project can evolve from requirements to executable software.

It focuses on:

- software component design,
- RTE-style communication,
- generated mapping concepts,
- runnable activation,
- scheduler behavior,
- diagnostics,
- persistence,
- testing,
- documentation,
- physical mapping from concepts to files.

## Project Scenario

A Tier-1 automotive supplier is developing a simplified ECU responsible for processing object-candidate data from a perception pipeline.

The ECU receives object candidates from an upstream perception or sensor-processing module. That upstream module is assumed to have already detected candidate objects using perception or tracking algorithms.

The MVP does not implement raw object detection or Kalman-filter-based estimation.

Instead, this ECU performs:

- rule-based object-candidate validation,
- diagnostic tracking,
- status reporting,
- persistence of selected counters,
- simplified alert output generation.

## Scope

### In Scope

- C++17 implementation
- CMake build system
- GoogleTest-ready test structure
- AUTOSAR-inspired SWC structure
- simulated generated RTE API
- simulated RTE mapping
- simulated scheduler / OS behavior
- simplified BSW modules
- simplified DCM / DEM / NvM concepts
- bounded object-candidate list processing
- diagnostic counters and status data
- object-alert output for future GPIO/MCU mapping
- Mermaid / Graphviz / PlantUML-ready architecture documentation
- LaTeX and Medium publication artifacts after phase acceptance

### Out of Scope for MVP

- production AUTOSAR compliance
- vendor-generated RTE
- real ARXML toolchain generation
- real CAN driver integration
- real STM32 execution
- raw sensor perception
- Kalman-filter-based tracking
- production ADAS algorithm performance

## MVP Algorithm Scope

The MVP is defined as:

```text
Object tracking = bounded object-candidate validation + diagnostic tracking
```

The ECU evaluates a bounded list of object candidates using:

- confidence,
- distance,
- object age,
- sensor quality.

For each cycle:

1. Read a bounded candidate list.
2. Validate sensor quality.
3. Validate confidence threshold.
4. Validate distance range.
5. Validate age/stability.
6. Accept, reject, or ignore each candidate.
7. Update tracking state, counters, diagnostic events, and alert output.

## Future Algorithm Extension

A Kalman filter, alpha-beta filter, or more advanced tracking algorithm may be added later.

This should be considered after the STM32 MCAL/HAL and FreeRTOS-related repositories have matured enough to support a more hardware-oriented and timing-aware implementation.

## Main Inputs

```text
ObjectCandidateList
VehicleSpeed
DiagnosticRequest
```

## Main Outputs

```text
TrackedObjectStatus
DiagnosticResponse
DiagnosticEventStatus
PersistentDiagnosticCounters
ObjectAlertOutput
```

## Main Internal State

```text
ActiveObjectState
FalsePositiveCounter
AcceptedObjectCounter
LastObjectConfidence
LastDiagnosticStatus
LastAcceptedObjectId
```

## Initial Software Components

### SensorInputSWC

Purpose:

```text
Provide a bounded list of simplified object candidates to the ECU.
```

Runnable:

```text
SensorInput_MainCycle()
```

Output:

```text
ObjectCandidateListOut
```

Physical file:

```text
src/application/SensorInputSWC.cpp
```

### ObjectTrackerSWC

Purpose:

```text
Evaluate object candidates, maintain tracking state, and report diagnostics.
```

Runnables:

```text
ObjectTracker_MainCycle()
ObjectTracker_DiagnosticRead()
```

Inputs:

```text
ObjectCandidateListIn
VehicleSpeedIn
```

Outputs:

```text
TrackedObjectStatusOut
ObjectAlertOutput
DiagnosticStatusOut
```

Physical file:

```text
src/application/ObjectTrackerSWC.cpp
```

### DiagnosticAdapter / DCM Concept

Purpose:

```text
Represent simplified diagnostic access to application and DEM/NvM data.
```

Physical file:

```text
src/bsw/Dcm.cpp
```

## Phase 1 Decisions

| Decision ID | Decision |
|---|---|
| DEC-001 | The MVP will not implement a Kalman filter. |
| DEC-002 | The ECU receives pre-computed object candidates from an upstream perception/sensor-processing module. |
| DEC-003 | The MVP object evaluation algorithm will use deterministic rule-based gating. |
| DEC-004 | A Kalman filter, alpha-beta filter, or more advanced tracking algorithm may be added as a future extension. |
| DEC-005 | Sensor quality shall be modeled as an enum: `Good`, `Degraded`, `Invalid`, `Timeout`. |
| DEC-006 | Object input shall be modeled as a bounded `ObjectCandidateList_T` for O(n) evaluation. |
| DEC-007 | Invalid or timeout sensor quality shall be reported through a DEM-style Client-Server/service call. |
| DEC-008 | The 10 ms runnable period is accepted only under a bounded candidate-list workload. |
| DEC-009 | A simple `ObjectAlertOutput` shall be added to support future GPIO/MCU mapping. |
| DEC-010 | Simulated RTE, BSW, and OS code are part of the executable educational ECU simulation, not only test scaffolding. |

## Phase 1 Status

```text
Phase: 1
Name: Project Concept and Engineering Context
Status: Updated Draft
Next Phase: Requirements Engineering
```
