# ADR-001: Three-Repository Layout

- Status: Accepted
- Decision: Keep shared specifications, backend implementation and frontend implementation in three separate Git repositories.
- Context: Independent AI agents need clear ownership while preserving one shared source of truth.
- Consequences:
  - Cross-repository rules and API contract are maintained in `hiring-assessment-specs`.
  - Backend and frontend remain independently buildable and testable.
  - All three repositories should be opened together in the Orca workspace.
  - API contract changes require coordinated updates and compatibility review.
