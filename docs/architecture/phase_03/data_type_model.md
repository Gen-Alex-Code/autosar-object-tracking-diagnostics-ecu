# Data Type Model

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 3 — SWC Architecture  
**Status:** Draft v3

---

## 1. Purpose

This document defines the initial data types used by the SWC architecture.

The final implementation will use C++17. The names intentionally resemble AUTOSAR-style generated RTE types.

---

## 2. Constants

```cpp
constexpr std::size_t kMaxObjectCandidates = 8U;
constexpr std::uint8_t kTentativeConfidenceThreshold = 50U;
constexpr std::uint8_t kActiveConfidenceThreshold = 70U;
constexpr std::uint8_t kDegradedConfidenceThreshold = 85U;
constexpr std::uint16_t kMinDistanceCm = 50U;
constexpr std::uint16_t kMaxDistanceCm = 20000U;
constexpr std::uint16_t kRegionOfInterestMaxCm = 10000U;
constexpr std::uint8_t kMinimumAgeCycles = 2U;
constexpr std::uint8_t kConfirmationWindowCycles = 3U;
constexpr std::uint8_t kRequiredDetectionsForActive = 2U;
constexpr std::uint8_t kMaxMissedCycles = 5U;
constexpr std::uint16_t kMainCyclePeriodMs = 10U;
```

---

## 3. Enumerations

```cpp
enum class SensorQuality_T : std::uint8_t
{
    Good,
    Degraded,
    Invalid,
    Timeout
};

enum class SensorSourceStatus_T : std::uint8_t
{
    Good,
    Degraded,
    Invalid,
    Timeout
};

enum class CandidateDecision_T : std::uint8_t
{
    Accepted,
    Rejected,
    Ignored
};

enum class TrackingState_T : std::uint8_t
{
    Inactive,
    Tentative,
    Active,
    Coasting,
    Lost
};

enum class ObjectAlertOutput_T : std::uint8_t
{
    Inactive,
    ObjectTracked,
    DiagnosticCondition
};

enum class RejectionReason_T : std::uint8_t
{
    None,
    LowConfidence,
    DistanceTooNear,
    DistanceTooFar,
    OutsideRegionOfInterest,
    InvalidCandidateQuality,
    TimeoutCandidateQuality,
    InvalidSourceStatus,
    TimeoutSourceStatus,
    MalformedCandidateList
};
```

---

## 4. Structures

```cpp
struct ObjectCandidate_T
{
    std::uint32_t object_id;
    std::uint16_t distance_cm;
    std::int16_t relative_velocity_cm_s;
    std::uint8_t confidence_percent;
    std::uint8_t age_cycles;
    SensorQuality_T quality;
    std::uint32_t timestamp_ms;
};

struct ObjectCandidateList_T
{
    std::array<ObjectCandidate_T, kMaxObjectCandidates> candidates;
    std::uint8_t candidate_count;
    SensorSourceStatus_T source_status;
    std::uint32_t timestamp_ms;
};

struct TrackedObjectStatus_T
{
    TrackingState_T tracking_state;
    bool primary_object_present;
    std::uint32_t primary_object_id;
    std::uint8_t last_confidence_percent;
    bool confidence_valid;
    std::uint8_t missed_cycles;
};

struct TrackingDiagnosticSnapshot_T
{
    TrackingState_T tracking_state;
    std::uint32_t primary_object_id;
    std::uint8_t last_confidence_percent;
    bool confidence_valid;
    std::uint8_t missed_cycles;
    std::uint32_t accepted_object_counter_total;
    std::uint32_t false_positive_counter_total;
    RejectionReason_T last_rejection_reason;
    SensorSourceStatus_T source_status;
    ObjectAlertOutput_T object_alert_state;
};
```

---

## 5. Expected Location

Most RTE-facing data types shall be placed in:

```text
include/generated/rte/Rte_Types.hpp
```

Internal helper classes may be placed later under:

```text
include/application/
src/application/
```

---

## 6. Traceability

| Data type | Requirement |
|---|---|
| `ObjectCandidate_T` | `REQ-SW-001` |
| `ObjectCandidateList_T` | `REQ-SW-002` |
| `SensorQuality_T` | `REQ-SW-003` |
| `SensorSourceStatus_T` | `REQ-SW-004` |
| `CandidateDecision_T` | `REQ-SW-005` |
| `TrackingState_T` | `REQ-SW-006` |
| `TrackedObjectStatus_T` | `REQ-SYS-003`, `REQ-DIAG-002` |
| `TrackingDiagnosticSnapshot_T` | `REQ-DIAG-008` |
