# autosar-object-tracking-diagnostics-ecu

## Purpose

This repository is the Portfolio Reference Architecture for an embedded systems portfolio.

It demonstrates an AUTOSAR Classic-inspired object tracking diagnostics ECU using C++17, CMake, simulated generated RTE, simplified BSW services, scheduler behavior, diagnostics, persistence, tests, and documentation.

## Main Goals

- Explain AUTOSAR concepts through physical files, functions, interfaces, and tests.
- Provide a realistic project workflow from concept to executable.
- Support LaTeX, Medium, and GitHub documentation.
- Guide specialized portfolio repositories.
- Create recruiter-facing technical evidence.

## Scope

This project can be used to demonstrate:

- C++17
- CMake
- Unit testing
- Embedded software architecture
- Documentation
- Requirements traceability

## Build

```bash
cmake -S . -B build
cmake --build build
ctest --test-dir build
```

## Status

Initial repository template created.
