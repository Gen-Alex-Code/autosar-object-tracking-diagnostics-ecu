# Requirements Index

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## 1. Purpose

This index defines the structure of the requirements package and the lightweight traceability mechanism used by the project.

The aim is to resemble a **professional, lightweight, DOORS-style workflow** while keeping the repository simple enough to maintain in Markdown.

---

## 2. Requirements Documents

| Document | Purpose |
|---|---|
| `system_requirements.md` | Externally visible ECU behavior. |
| `software_requirements.md` | Data types, SWC behavior, RTE access, persistence, implementation constraints. |
| `diagnostic_requirements.md` | DEM-style events, DCM-style reads, counters, reset behavior. |
| `timing_requirements.md` | Runnable period, scheduler behavior, runtime constraints. |
| `tracking_lifecycle_requirements.md` | Object lifecycle state machine and transition rules. |
| `validation_policy.md` | Candidate validation thresholds, status classification, primary-object selection. |
| `traceability_matrix.md` | Mapping to design, code, tests, and research references. |

---

## 3. Future Verification Documents

A dedicated test requirement or verification document is planned for a later phase.

Phase 2 defines the expected verification method for each requirement, but the detailed test plan will be drafted when the design and implementation structure are stable.

Planned future documents:

| Future document | Expected phase | Purpose |
|---|---|---|
| `docs/tests/test_strategy.md` | Verification planning phase | Defines unit, integration, timing, and diagnostic test strategy. |
| `docs/tests/test_cases.md` | Implementation / verification phase | Lists concrete test cases mapped to requirement IDs. |
| `docs/tests/test_report.md` | Verification phase | Records executed test evidence and results. |

---

## 4. Requirement ID Convention

| Prefix | Meaning |
|---|---|
| `REQ-SYS` | System-level requirement |
| `REQ-SW` | Software requirement |
| `REQ-VAL` | Candidate validation requirement |
| `REQ-TRACK` | Tracking lifecycle requirement |
| `REQ-DIAG` | Diagnostic requirement |
| `REQ-TIME` | Timing / scheduling requirement |
| `REQ-NF` | Non-functional requirement |
| `REQ-TRACE` | Traceability requirement |

---

## 5. Requirement Metadata Template

```markdown
## REQ-XXX-000 — Requirement Title

**Status:** Draft / Approved / Deprecated  
**Owner:** Role or discipline  
**Rationale:** Why the requirement exists  
**Verification:** Unit test / integration test / timing test / review  
**Related design:** Link to design document section  
**Related research:** Link to research reference, if applicable  
**Code trace:** Future source file or function  
**Test trace:** Future test file or test case  

The system/software shall...
```

---

## 6. Code and Test Traceability Convention

Implementation comment:

```cpp
// REQ-TRACK-002
// Transition Inactive -> Tentative when a candidate passes the initial gate.
```

Test comment:

```cpp
// Verifies: REQ-TRACK-002
TEST(TrackingLifecycleTest, InactiveCandidateBecomesTentative)
{
    // test
}
```

---

## 7. Lightweight Traceability Principle

Traceability shall be useful but not heavy.

The project shall trace:

```text
requirement → functional concept → design element → source file/function → test case → review evidence
```

The goal is to support impact analysis without turning the repository into an administrative burden.
