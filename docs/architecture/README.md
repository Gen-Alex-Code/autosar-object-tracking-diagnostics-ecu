# Architecture

Architecture artifacts are organized by phase because some diagrams become progressively more detailed.

## Folder Strategy

```text
docs/architecture/
├── phase_01/
│   ├── portfolio_context.mmd
│   ├── project_workflow.mmd
│   ├── software_architecture_initial.mmd
│   └── system_sequence_initial.mmd
└── current/
    ├── software_architecture.mmd
    └── system_sequence.mmd
```

## Policy

- Phase folders preserve the architecture view at the time of each phase.
- The `current/` folder may contain the latest approved version.
- Mermaid is the default diagram source format.
- Graphviz/DOT or PlantUML may be used when they produce more precise diagrams.
- Rendered PNG/SVG files may be added for LaTeX, Medium, or PDF publication.
