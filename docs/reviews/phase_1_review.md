# Phase 1 Review — Project Concept and Engineering Context

## Review Status

Ready for final review.

## Scope Reviewed

- Project purpose
- MVP scope
- Initial functional requirements
- Initial non-functional requirements
- Initial architecture diagrams
- Initial sequence diagram
- RTE / BSW / OS simulation strategy
- Documentation structure
- Publication artifacts

## Artifacts Produced

| Artifact | Path | Status |
|---|---|---|
| Project concept | `docs/concept/project_concept.md` | Updated Draft |
| Governance ADR | `docs/governance/ADR-001-portfolio-reference-architecture.md` | Accepted |
| System requirements | `docs/requirements/system_requirements.md` | Draft |
| Software requirements | `docs/requirements/software_requirements.md` | Draft |
| Traceability matrix | `docs/requirements/traceability_matrix.md` | Draft |
| Portfolio context diagram | `docs/architecture/phase_01/portfolio_context.mmd` | Updated Draft |
| Project workflow diagram | `docs/architecture/phase_01/project_workflow.mmd` | Draft |
| Software architecture diagram | `docs/architecture/phase_01/software_architecture_initial.mmd` | Updated Draft |
| System sequence diagram | `docs/architecture/phase_01/system_sequence_initial.mmd` | Draft |
| LaTeX publication artifact | `publication/latex/phase_01_project_concept.tex` | Draft |
| Medium publication artifact | `publication/medium/phase_01_project_concept.md` | Draft |

## Decisions

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
| DEC-011 | Portfolio-oriented explanation shall be minimized in repository-facing documents and kept mainly under governance. |
| DEC-012 | Architecture diagrams shall be versioned by phase when they are expected to evolve. |

## Open Questions

| ID | Question | Owner | Status |
|---|---|---|---|
| OQ-001 | Exact value of `N_MAX` for candidate-list processing | Software Architect | Open for Phase 2 |
| OQ-002 | Exact confidence threshold and distance range | Requirements Engineer | Open for Phase 2 |
| OQ-003 | Exact diagnostic DID list | Requirements Engineer / Diagnostic Owner | Open for Phase 2 |
| OQ-004 | Whether `SensorInputSWC` remains a permanent SWC or becomes a test/upstream adapter | Software Architect | Open for Phase 2 |

## Gate Status

```text
Phase 1: READY FOR FINAL REVIEW
Next Phase: Phase 2 — Requirements Engineering
```
