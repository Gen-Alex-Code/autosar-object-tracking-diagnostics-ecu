# Runnable Model

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 3 — SWC Architecture  
**Status:** Draft v3

---

## 1. Purpose

This document defines the runnables associated with the two Application Layer SWCs.

---

## 2. Runnable Summary

| Runnable | SWC | Trigger | Period / Condition | Purpose |
|---|---|---|---|---|
| `SensorInput_MainCycle` | `SensorInputSWC` | Timing event | 10 ms | Produces or updates candidate list. |
| `ObjectTracker_MainCycle` | `ObjectTrackerSWC` | Timing event | 10 ms | Validates candidates and updates tracking lifecycle. |
| `ObjectTracker_DiagnosticRead` | `ObjectTrackerSWC` | Operation invoked event | Diagnostic request | Provides selected diagnostic read data. |

---

## 3. Main Cycle Execution Order

The MVP uses one deterministic 10 ms task. The runnables are not intended to execute simultaneously.

Initial scheduler order:

```text
Task_10ms
  ├── SensorInput_MainCycle()
  └── ObjectTracker_MainCycle()
```

Meaning:

```text
SensorInput_MainCycle() runs first within the 10 ms activation.
ObjectTracker_MainCycle() runs immediately afterward in the same 10 ms activation.
ObjectTracker_MainCycle() is not intentionally delayed by a full 10 ms cycle.
```

Rationale:

```text
SensorInputSWC updates the candidate list before ObjectTrackerSWC reads it.
```

If a later OS/RTE design maps the runnables to separate tasks with the same period, then priority, activation order, and data age shall be explicitly defined in the scheduler design phase.

---

## 4. ObjectTracker_MainCycle Responsibilities

`ObjectTracker_MainCycle()` shall:

```text
read ObjectCandidateList_T through RTE
validate list metadata
validate candidate entries
assign candidate decisions
select primary object
update tracking lifecycle
report diagnostic events if required
update counters if required
write TrackedObjectStatus_T
write ObjectAlertOutput_T
```

---

## 5. ObjectTracker_DiagnosticRead Responsibilities

`ObjectTracker_DiagnosticRead()` shall provide read-only access to selected values:

```text
tracking state
primary object ID
confidence information
counter values
last rejection reason
last diagnostic event
source status
alert state
```

It shall not modify the tracking lifecycle.

---

## 6. RTE Access Preview

```cpp
void ObjectTracker_MainCycle(void)
{
    ObjectCandidateList_T candidate_list{};

    Rte_Read_ObjectCandidateList(&candidate_list);

    // candidate validation and lifecycle update

    Rte_Write_TrackedObjectStatus(&status);
    Rte_Write_ObjectAlertOutput(alert);
}
```

---

## 7. Runnable Traceability

| Runnable | Requirements |
|---|---|
| `SensorInput_MainCycle` | `REQ-SYS-001`, `REQ-TIME-005` |
| `ObjectTracker_MainCycle` | `REQ-SYS-002`, `REQ-SYS-003`, `REQ-VAL-*`, `REQ-TRACK-*`, `REQ-TIME-001` |
| `ObjectTracker_DiagnosticRead` | `REQ-DIAG-002`, `REQ-DIAG-004`, `REQ-DIAG-008` |
