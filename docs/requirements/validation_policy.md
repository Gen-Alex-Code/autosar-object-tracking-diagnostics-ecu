# Validation Policy

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v2

---

## 1. Purpose

This document defines how candidate fields are used during execution.

The goal is deterministic behavior. The same candidate list and configuration shall produce the same candidate decisions, primary object selection, tracking lifecycle updates, and diagnostic outputs.

---

## 2. Locked Default Parameters

| Parameter | Default | Purpose |
|---|---:|---|
| `kMaxObjectCandidates` | `8` | Bounds runtime and memory. |
| `kTentativeConfidenceThreshold` | `50%` | Candidate may enter tentative state. |
| `kActiveConfidenceThreshold` | `70%` | Candidate may become active. |
| `kDegradedConfidenceThreshold` | `85%` | Higher threshold for degraded quality. |
| `kMinDistanceCm` | `50 cm` | Reject physically invalid near objects. |
| `kMaxDistanceCm` | `20000 cm` | Reject physically invalid far objects. |
| `kRegionOfInterestMaxCm` | `10000 cm` | Ignore valid but non-relevant far objects. |
| `kMinimumAgeCycles` | `2` | Prevent one-cycle noise from becoming active. |
| `kConfirmationWindowCycles` | `3` | Window for active confirmation. |
| `kRequiredDetectionsForActive` | `2` | M-out-of-N confirmation. |
| `kMaxMissedCycles` | `5` | Coasting deletion threshold. |
| `kMainCyclePeriodMs` | `10 ms` | Runnable period. |

---

## 3. Validation Order

Candidate processing shall follow this order:

```text
1. Validate candidate list count.
2. Validate source status.
3. Validate candidate sensor quality.
4. Validate physical distance range.
5. Validate region of interest.
6. Validate confidence threshold.
7. Validate age/stability.
8. Assign candidate decision.
9. Select primary object.
10. Update tracking lifecycle.
```

The candidate decision changes at step 8. The lifecycle state changes at step 10.

---

## 4. Feature Usage

| Feature | Used when | Effect |
|---|---|---|
| `candidate_count` | Before candidate loop | Defines how many entries are valid for this cycle. |
| `source_status` | Before candidate loop | Can skip the full list and report source-level diagnostic event. |
| `timestamp_ms` | Before candidate loop | Supports freshness/timeout checks. |
| `quality` | Per candidate | Invalid/Timeout causes rejection and diagnostic event. |
| `distance_cm` | Per candidate | Physical invalid range causes rejection; outside ROI causes ignored. |
| `confidence_percent` | Per candidate | Determines tentative/active eligibility. |
| `age_cycles` | Per candidate | Prevents one-cycle noise from becoming active. |
| `relative_velocity_cm_s` | Primary-object selection | Used as risk-ranking tie-breaker. |
| `object_id` | Lifecycle association | Used to reconnect objects within coasting window. |

---

## REQ-VAL-001 — Candidate Count Validation

The ECU shall process only candidate indices from `0` to `candidate_count - 1`.

If `candidate_count > kMaxObjectCandidates`, the ECU shall safely reject the list and report a diagnostic event.

---

## REQ-VAL-002 — Empty List Handling

An empty candidate list shall not cause crash, freeze, undefined state, or out-of-bounds access.

Empty list behavior:

```text
no candidate is accepted
no candidate is rejected
tracking lifecycle updates according to missed-detection policy
runnable completes normally
```

---

## REQ-VAL-003 — Partially Filled List Handling

A partially filled candidate list shall process only valid entries from index `0` to `candidate_count - 1`.

Unused array entries shall not be read.

---

## REQ-VAL-004 — Source Status Validation

If `source_status` is `Invalid` or `Timeout`, the ECU shall skip candidate-level processing and report a DEM-style source-status event.

---

## REQ-VAL-005 — Candidate Quality Validation

If a candidate quality is `Invalid` or `Timeout`, the candidate shall be classified as `Rejected`.

The ECU shall report or update the corresponding diagnostic behavior.

---

## REQ-VAL-006 — Distance Validation

A candidate shall be classified as `Rejected` if its distance is outside the physical valid range.

A candidate shall be classified as `Ignored` if its distance is physically valid but outside the region of interest.

---

## REQ-VAL-007 — Confidence Validation

A candidate may become eligible for `Accepted` only if its confidence is equal to or greater than the active confidence threshold.

A degraded-quality candidate shall require the degraded confidence threshold.

---

## REQ-VAL-008 — Age / Stability Validation

A candidate shall not become active unless its `age_cycles` value is greater than or equal to `kMinimumAgeCycles`.

---

## REQ-VAL-009 — Accepted Candidate Decision

A candidate shall be classified as `Accepted` when it satisfies the configured source, quality, distance, region-of-interest, confidence, and age/stability rules.

---

## REQ-VAL-010 — Rejected Candidate Decision

A candidate shall be classified as `Rejected` when the candidate indicates invalid, malformed, or diagnostically relevant unreliable data.

Rejected candidates may update rejection counters or diagnostic events.

---

## REQ-VAL-011 — Ignored Candidate Decision

A candidate shall be classified as `Ignored` when it is not faulty but should not affect primary tracking.

Ignored candidates shall not increment the false-positive counter.

---

## REQ-VAL-012 — Primary Object Selection

If multiple accepted candidates exist, the ECU shall select one primary object using:

```text
1. shortest valid distance
2. highest confidence
3. highest closing velocity
4. lowest object_id
```
