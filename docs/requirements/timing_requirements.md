# Timing Requirements

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## REQ-TIME-001 — Main Runnable Period

**Status:** Draft  
**Owner:** Software Architect  
**Verification:** Scheduler integration test  

The ECU shall execute `ObjectTracker_MainCycle()` every 10 ms in the simulated scheduler.

The 10 ms value is a project timing decision, not an AUTOSAR requirement.

---

## REQ-TIME-002 — Bounded Candidate Count

**Status:** Draft  
**Owner:** Software Architect  
**Verification:** Boundary test  

The main runnable shall process at most `kMaxObjectCandidates` entries per cycle.

Initial value:

```text
kMaxObjectCandidates = 8
```

---

## REQ-TIME-003 — Bounded Runtime Complexity

**Status:** Draft  
**Owner:** Software Architect / Developer  
**Verification:** Code review + unit test  

The candidate evaluation algorithm shall have O(n) runtime complexity, where `n` is the number of valid candidates in the bounded list.

This requirement controls algorithmic growth.

---

## REQ-TIME-004 — Runnable Completion Budget

**Status:** Draft  
**Owner:** Software Architect / Test Engineer  
**Verification:** Timing measurement test  

`ObjectTracker_MainCycle()` shall complete within the 10 ms execution period under the supported MVP workload.

This requirement is related to `REQ-TIME-002` and `REQ-TIME-003`, but it is distinct:

```text
REQ-TIME-002:
    limits how many candidates can be processed

REQ-TIME-003:
    limits the algorithmic complexity

REQ-TIME-004:
    verifies that the actual runnable completes within the selected period
```

---

## REQ-TIME-005 — Deterministic Runnable Order

**Status:** Draft  
**Owner:** Software Architect  
**Verification:** Scheduler integration test  

The 10 ms task shall execute runnable-related functions in a deterministic order.

Initial order:

```text
SensorInput_MainCycle()
ObjectTracker_MainCycle()
diagnostic/service update points as required
```

---

## REQ-TIME-006 — No Dynamic Allocation in Main Cycle

**Status:** Draft  
**Owner:** Software Architect / Developer  
**Verification:** Code review + static-analysis review  

The main tracking cycle shall not perform dynamic memory allocation.

Prohibited inside `ObjectTracker_MainCycle()`:

```cpp
new
delete
malloc
free
std::vector::push_back on dynamically allocated storage
```

Fixed-size storage and stack-local objects are preferred for the MVP.
