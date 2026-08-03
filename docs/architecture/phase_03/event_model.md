# Event Model

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 3 — SWC Architecture  
**Status:** Draft v3

---

## 1. Purpose

This document defines the events that activate runnables in the Phase 3 SWC architecture.

---

## 2. Event Types

| Event | AUTOSAR-inspired concept | Runnable | Purpose |
|---|---|---|---|
| `EVT_10MS_SENSOR_INPUT` | Timing event | `SensorInput_MainCycle` | Update candidate list every 10 ms. |
| `EVT_10MS_OBJECT_TRACKER` | Timing event | `ObjectTracker_MainCycle` | Execute tracker validation/lifecycle every 10 ms. |
| `EVT_DIAG_READ_TRACKER_DATA` | Operation invoked event | `ObjectTracker_DiagnosticRead` | Diagnostic read of selected tracking information. |

---

## 3. Timing Events and Execution Order

The 10 ms timing events are project decisions used to emulate cyclic ECU execution.

Both main runnables are modeled with 10 ms timing behavior, but the MVP implementation shall execute them sequentially inside the same 10 ms simulated task.

```text
10 ms activation
↓
SensorInput_MainCycle()
↓
ObjectTracker_MainCycle()
```

This means the tracker consumes the candidate list produced in the same scheduler activation.

The architecture does not require simultaneous execution. If future OS mapping uses independent tasks, execution order and data freshness become explicit scheduler-design decisions.

---

## 4. Operation Invoked Events

Diagnostic reads are modeled as operation-invoked events.

```text
Diagnostic request
↓
DCM-style BSW path
↓
RTE diagnostic read operation
↓
ObjectTracker_DiagnosticRead()
↓
response data
```

---

## 5. Traceability

| Event | Requirements |
|---|---|
| `EVT_10MS_SENSOR_INPUT` | `REQ-TIME-001`, `REQ-TIME-005` |
| `EVT_10MS_OBJECT_TRACKER` | `REQ-TIME-001`, `REQ-TIME-005` |
| `EVT_DIAG_READ_TRACKER_DATA` | `REQ-DIAG-002`, `REQ-DIAG-004`, `REQ-DIAG-008` |
