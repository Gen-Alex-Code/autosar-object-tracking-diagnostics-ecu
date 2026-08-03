# Research Index

**Project:** `autosar-object-tracking-diagnostics-ecu`  
**Phase:** 2 — Requirements Engineering  
**Status:** Draft v3

---

## 1. Purpose

This document keeps external references connected to the project requirements.

The goal is not to turn the repository into a research paper. The goal is to support engineering choices with clear references and make the rationale traceable.

---

## REF-001 — Simple Online and Realtime Tracking

**Title:** Simple Online and Realtime Tracking  
**Authors:** Alex Bewley, Zongyuan Ge, Lionel Ott, Fabio Ramos, Ben Upcroft  
**Source:** arXiv:1602.00763  
**Use in this project:** Supports the idea that simple real-time tracking approaches can be valuable when detection quality, online operation, and computational efficiency are prioritized.

Applied to:

```text
REQ-SYS-002
REQ-SYS-006
REQ-VAL-007
REQ-VAL-012
REQ-TIME-003
```

Link:

```text
https://arxiv.org/abs/1602.00763
```

---

## REF-002 — Introduction to Multiple Target Tracking

**Source:** MathWorks Sensor Fusion and Tracking documentation  
**Use in this project:** Supports concepts such as track confirmation, track deletion, missed detections, and lifecycle-style track management.

Applied to:

```text
REQ-SYS-003
REQ-SYS-004
REQ-SYS-007
REQ-TRACK-003
REQ-TRACK-006
REQ-TRACK-008
REQ-VAL-008
```

Link:

```text
https://www.mathworks.com/help/fusion/ug/introduction-to-multiple-target-tracking.html
```

---

## REF-003 — Multi-Object Tracker Block Documentation

**Source:** MathWorks Automated Driving / Sensor Fusion documentation  
**Use in this project:** Supports the idea of initializing, confirming, predicting/correcting, and deleting tracks, including M-out-of-N style logic.

Applied to:

```text
REQ-SW-002
REQ-SW-006
REQ-TRACK-003
REQ-TRACK-008
```

Link:

```text
https://www.mathworks.com/help/driving/ref/multiobjecttracker.html
```

---

## REF-004 — SFSORT: Scene Features-based Simple Online Real-Time Tracker

**Title:** SFSORT: Scene Features-based Simple Online Real-Time Tracker  
**Source:** arXiv:2404.07553  
**Use in this project:** Supports awareness that modern real-time tracking research may prioritize computational simplicity and may even remove Kalman filtering under specific approaches.

Applied to:

```text
REQ-SYS-002
REQ-VAL-007
REQ-VAL-012
REQ-TIME-003
```

Link:

```text
https://arxiv.org/abs/2404.07553
```

---

## REF-005 — AUTOSAR Classic Platform Layering

**Source:** AUTOSAR Classic Platform overview  
**Use in this project:** Supports the educational architecture split between Application Layer, RTE, and BSW.

Applied to:

```text
REQ-SYS-005
REQ-SW-007
REQ-DIAG-001
REQ-DIAG-005
```

Link:

```text
https://www.autosar.org/standards/classic-platform
```

---

## REF-006 — AUTOSAR RTE Software Specification

**Source:** AUTOSAR Classic Platform RTE specification  
**Use in this project:** Supports the simulated RTE and scheduler-glue learning model.

Applied to:

```text
REQ-SW-007
REQ-TIME-001
```

Link:

```text
https://www.autosar.org/fileadmin/standards/R24-11/CP/AUTOSAR_CP_SWS_RTE.pdf
```

---

## REF-007 — AUTOSAR Runnable and Event Modeling

**Source:** AUTOSAR runnable/event configuration references  
**Use in this project:** Supports the concept that runnable activation can be modeled through timing events and scheduler integration.

Applied to:

```text
REQ-TIME-001
REQ-TIME-007
```

Link:

```text
https://www.mathworks.com/help/autosar/ug/configure-autosar-runnables-and-events.html
```

---

## 2. Research Usage Policy

Research references support rationale. They do not create production-compliance claims.
