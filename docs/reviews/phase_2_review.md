# Phase 2 Review — Requirements Engineering

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2  
**Name:** Requirements Engineering  
**Status:** Draft v2 ready for review  
**Review type:** Internal engineering review

---

## 1. Review Purpose

This review records the Phase 2 requirements-engineering output before moving into SWC architecture and implementation planning.

---

## 2. Scope Reviewed

Phase 2 defines:

```text
functional concept
requirement structure
lightweight traceability mechanism
system requirements
software requirements
tracking lifecycle state machine
validation policy
diagnostics requirements
timing requirements
research reference index
traceability matrix
```

---

## 3. Artifacts Produced

```text
docs/design/functional_concept.md
docs/requirements/requirements_index.md
docs/requirements/system_requirements.md
docs/requirements/software_requirements.md
docs/requirements/diagnostic_requirements.md
docs/requirements/timing_requirements.md
docs/requirements/tracking_lifecycle_requirements.md
docs/requirements/validation_policy.md
docs/requirements/traceability_matrix.md
docs/research/research_index.md
docs/reviews/phase_2_review.md
```

Publication artifacts are intentionally not listed in this repository review record. LaTeX and Medium artifacts may be produced separately after the repository requirements package is accepted.

---

## 4. Existing Artifacts Updated

```text
docs/design/functional_concept.md
docs/requirements/requirements_index.md
docs/requirements/system_requirements.md
docs/requirements/software_requirements.md
docs/requirements/diagnostic_requirements.md
docs/requirements/timing_requirements.md
docs/requirements/tracking_lifecycle_requirements.md
docs/requirements/validation_policy.md
docs/requirements/traceability_matrix.md
docs/research/research_index.md
```

---

## 5. Key Decisions

| Decision | Value |
|---|---|
| MVP meaning | Minimum Viable Product |
| Maximum candidates | `8` |
| Main cycle period | `10 ms` |
| Candidate decisions | `Accepted`, `Rejected`, `Ignored` |
| Tracking states | `Inactive`, `Tentative`, `Active`, `Coasting`, `Lost` |
| Confirmation rule | `2 detections within 3 cycles` |
| Deletion rule | `5 consecutive missed cycles` |
| Object ID persistence | RAM-only |
| Confidence persistence | RAM-only |
| Persistent counters | False-positive total and accepted-object total |
| Alert output | Application-level output, not direct GPIO |
| Primary object policy | Deterministic risk-priority selection |
| Diagnostic snapshot | Selected engineering/debug view, not full internal dump |
| Code quality tools | clang-format planned; clang-tidy/sanitizers recommended for host builds |
| Package manager | CMake FetchContent for GoogleTest in MVP |

---

## 6. Review Notes Incorporated

```text
MVP acronym defined.
Candidate decision change point clarified.
DOORS-style wording refined.
Future test requirement documents clarified.
Candidate decisions separated from lifecycle states.
Rejection tracking connected to diagnostics.
Primary object selection made more explicit.
Diagnostic status, confidence, and snapshot reads differentiated.
ObjectCandidateList fields explained.
SensorSourceStatus purpose and users defined.
Code quality tooling considerations added.
Timing requirement overlap clarified.
Publication artifacts removed from the Phase 2 review artifact list.
```

---

## 7. Gate Status

```text
Phase 2 status: Draft v2 ready for review
Recommended next action: Review the revised requirements package and approve or adjust thresholds/policies.
```
