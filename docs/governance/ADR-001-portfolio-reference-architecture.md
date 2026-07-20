# ADR-001 — Portfolio Reference Architecture Adoption

## Status

Accepted

## Date

2026-06-29

## Decision

The Embedded Systems Portfolio will adopt `autosar-object-tracking-diagnostics-ecu` as the Portfolio Reference Architecture.

## Context

The broader portfolio contains multiple specialized repositories:

- `autosar-ecu-diagnostics-demo`
- STM32 Peripheral Driver Framework
- FreeRTOS Task Scheduler Demo
- Automotive CAN Communication Stack
- Embedded Linux Gateway Simulator
- Embedded Systems Portfolio Hub
- future projects

A common architectural reference is needed to align learning, documentation, and implementation decisions across these repositories.

## Architecture

```text
                           Learning
                               │
                               ▼
               Portfolio Reference Architecture
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
 autosar-ecu-...        STM32 Drivers        FreeRTOS Demo
          │                    │                    │
          ├──────────────┬─────┴──────────────┐
          ▼              ▼                    ▼
 CAN Stack      Embedded Linux Gateway   Future Projects
          └──────────────┬────────────────────┘
                         ▼
          Embedded Systems Portfolio Hub
```

## Definition

```text
Portfolio Reference Architecture = autosar-object-tracking-diagnostics-ecu
```

## Purpose

The Portfolio Reference Architecture defines the shared engineering model used to align the specialized repositories.

It defines:

- development workflow,
- role-based engineering process,
- requirements and traceability structure,
- AUTOSAR-inspired SWC design,
- RTE-style API and generated mapping concepts,
- OS and scheduler interaction,
- BSW service concepts,
- diagnostic and persistence concepts,
- file structure,
- implementation patterns,
- testing strategy,
- documentation strategy.

## Repository Boundary

This ADR is intentionally stored under `docs/governance/`.

Most readers interested only in the repository implementation should start with:

```text
README.md
docs/concept/project_concept.md
docs/requirements/system_requirements.md
docs/architecture/phase_01/software_architecture_initial.mmd
```

This ADR exists mainly to document why this repository is part of a larger portfolio architecture.

## Consequences

### Positive Consequences

- The project gains a clear role in the larger portfolio.
- Specialized repositories can reuse architectural decisions.
- The reference architecture avoids duplicating hardware-specific work from other repositories.

### Tradeoffs

- Governance documentation must not dominate implementation documentation.
- Repository-facing documents should remain useful even for readers who do not care about the full portfolio.

## Status

Accepted.
