# Diagnostic Requirements

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## 1. Diagnostic Scope

Diagnostics shall explain tracker behavior and input-quality problems.

The diagnostic model is not intended to identify every real-world object disappearance. A disappearing object is normally handled by the tracking lifecycle (`Active → Coasting → Lost`) unless the disappearance is caused by an invalid or timeout source condition.

---

## REQ-DIAG-001 — Invalid Candidate Quality Event

**Status:** Draft  
**Owner:** Diagnostics Developer  
**Verification:** DEM event unit test  

The ECU shall report a DEM-style event when an individual candidate has quality `Invalid` or `Timeout`.

---

## REQ-DIAG-002 — Tracking Status Read

**Status:** Draft  
**Owner:** Diagnostics Developer  
**Verification:** DCM read test  

The ECU shall expose the current tracking status through a simplified DCM-style diagnostic read.

Purpose:

```text
Provide a compact read of the current lifecycle state and primary-object presence.
```

Initial content:

```text
tracking_state
primary_object_present
primary_object_id
```

This requirement is intentionally narrower than the diagnostic snapshot.

---

## REQ-DIAG-003 — False Positive Counter Read

**Status:** Draft  
**Owner:** Diagnostics Developer  
**Verification:** Counter read test  

The ECU shall expose a persistent cumulative false-positive counter.

Meaning:

```text
total number of candidates rejected by validation logic since the last diagnostic clear/reset
```

---

## REQ-DIAG-004 — Last Object Confidence Read

**Status:** Draft  
**Owner:** Diagnostics Developer  
**Verification:** DCM read test  

The ECU shall expose the confidence value associated with the current or most recently active primary object.

Purpose:

```text
Provide a focused read of confidence behavior without requiring the full diagnostic snapshot.
```

If no active or coasting object exists, the read shall return:

```text
confidence = 0
confidence_valid = false
```

This requirement is related to `REQ-DIAG-002`, but it is not identical. `REQ-DIAG-002` reports lifecycle state. `REQ-DIAG-004` reports confidence information.

---

## REQ-DIAG-005 — Source Status Event

**Status:** Draft  
**Owner:** Diagnostics Developer  
**Verification:** Source-status diagnostic test  

The ECU shall report a DEM-style event when `SensorSourceStatus_T` is `Invalid` or `Timeout`.

When this event occurs, candidate processing shall be skipped for the current cycle.

---

## REQ-DIAG-006 — Accepted Object Counter

**Status:** Draft  
**Owner:** Diagnostics Developer  
**Verification:** Counter update test  

The ECU shall maintain a persistent cumulative accepted-object counter.

The counter shall increase when a candidate contributes to a confirmed active object according to the lifecycle policy.

---

## REQ-DIAG-007 — Diagnostic Clear / Reset

**Status:** Draft  
**Owner:** Diagnostics Developer  
**Verification:** Diagnostic reset test  

The ECU shall support a simplified diagnostic reset behavior for persistent counters.

The reset behavior shall clear:

```text
false_positive_counter_total
accepted_object_counter_total
selected diagnostic event history
```

The reset behavior shall not clear current RAM-only lifecycle state unless explicitly requested by a future requirement.

---

## REQ-DIAG-008 — Diagnostic Snapshot Read

**Status:** Draft  
**Owner:** Diagnostics Developer / Test Engineer  
**Verification:** Snapshot read test  

The ECU shall expose a diagnostic snapshot for engineering visibility.

The snapshot shall not expose every internal variable. It shall expose selected values that explain the current tracking behavior.

Initial snapshot content:

```text
tracking_state
primary_object_id
last_confidence_percent
confidence_valid
missed_cycles
accepted_object_counter_total
false_positive_counter_total
last_rejection_reason
last_diagnostic_event
source_status
object_alert_state
```

Purpose:

```text
support debugging
support requirement review
support integration testing
explain why the tracker is Active, Coasting, Lost, or reporting a diagnostic condition
```
