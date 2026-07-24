# System Requirements

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## REQ-SYS-001 — Candidate Input Reception

**Status:** Draft  
**Owner:** Requirements Engineer / Software Architect  
**Verification:** Integration test + boundary test  

The ECU shall receive a bounded list of object candidates from an upstream perception or sensor-processing module.

The candidate list shall support up to `kMaxObjectCandidates` entries.

The ECU shall process:

```text
empty candidate lists
partially filled candidate lists
full candidate lists
invalid oversized candidate lists
```

Empty or partially filled lists shall not cause crash, freeze, undefined behavior, or out-of-bounds access.

---

## REQ-SYS-002 — Candidate Validation

**Status:** Draft  
**Owner:** Requirements Engineer / Application SWC Developer  
**Verification:** Unit test  

The ECU shall evaluate each object candidate using confidence, distance, age/stability, candidate sensor quality, and source status.

Each processed candidate shall be assigned one of the following per-cycle decisions:

```text
Accepted
Rejected
Ignored
```

These decisions describe the result of validating an individual candidate during the current runnable cycle. They are **not** the same as the tracking lifecycle states.

Relationship:

```text
Candidate decision:
    Accepted / Rejected / Ignored

Tracking lifecycle state:
    Inactive / Tentative / Active / Coasting / Lost
```

An `Accepted` candidate may influence the tracking lifecycle. A `Rejected` candidate may update diagnostics. An `Ignored` candidate shall not update diagnostics and shall not directly modify the primary tracked object.

---

## REQ-SYS-003 — Active Object State

**Status:** Draft  
**Owner:** Software Architect  
**Verification:** Lifecycle integration test  

The ECU shall maintain an active object state when a validated candidate satisfies the tracking lifecycle confirmation policy.

The ECU shall maintain the object as active until:

```text
the associated object is missed,
the lifecycle enters Coasting,
the missed-cycle threshold is exceeded,
and the lifecycle reaches Lost.
```

The active object state shall be RAM-only and shall not be persisted across ECU startup.

---

## REQ-SYS-004 — Rejection Tracking

**Status:** Draft  
**Owner:** Requirements Engineer / Diagnostics Developer  
**Verification:** Unit test + diagnostic integration test  

The ECU shall track rejected candidates using rejection reasons and diagnostic counters.

This requirement supports the diagnostic development by providing explainable causes for candidate rejection, such as:

```text
low confidence
out-of-range distance
invalid candidate quality
timeout candidate quality
invalid source status
timeout source status
malformed candidate list
```

Rejected candidates may update diagnostic counters and may trigger DEM-style events according to the diagnostic requirements.

---

## REQ-SYS-005 — Object Alert Output

**Status:** Draft  
**Owner:** Software Architect / MCU-MCAL Developer  
**Verification:** RTE output test  

The ECU shall provide an application-level `ObjectAlertOutput_T`.

The alert output shall not directly call GPIO, PWM, CAN, or other low-level hardware functions from the application SWC.

The alert output shall be written through an RTE-style output and may later be mapped to hardware or communication interfaces by lower layers.

---

## REQ-SYS-006 — Primary Object Selection

**Status:** Draft  
**Owner:** Software Architect / Application SWC Developer  
**Verification:** Multi-candidate unit test  

If more than one candidate is accepted during the same cycle, the ECU shall select one candidate as the primary object.

The primary object shall be selected using the deterministic priority order defined in `validation_policy.md`:

```text
1. shortest valid distance
2. highest confidence
3. highest closing velocity
4. lowest object_id as deterministic tie-breaker
```

Only the primary object shall drive the main tracking lifecycle and `ObjectAlertOutput_T`.

Accepted non-primary candidates may be counted as valid accepted candidates but shall not replace the primary object unless the selection policy chooses them in a later cycle.

---

## REQ-SYS-007 — Reappearance Handling

**Status:** Draft  
**Owner:** Software Architect / Test Engineer  
**Verification:** Lifecycle reappearance test  

The ECU shall treat a reappearing candidate as the same runtime object only if it can be associated within the configured coasting window.

Initial MVP association rule:

```text
same object_id within kMaxMissedCycles
```

After the lifecycle reaches `Lost`, a later candidate shall be treated as a new runtime tracking event even if the upstream `object_id` value is reused.

---

## REQ-SYS-008 — Functional Diagnostic Visibility

**Status:** Draft  
**Owner:** Requirements Engineer / Diagnostics Developer  
**Verification:** Diagnostic read test  

The ECU shall expose enough diagnostic information to explain the current tracking behavior.

The diagnostic information shall include selected lifecycle, confidence, counter, and rejection data. It shall not be treated as a raw dump of every internal variable.
