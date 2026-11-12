# Hiring Assessment — Shared Specs

This repository is the shared context and contract source for the three-repository project.

## Repositories
- `hiring-assessment-specs`: requirements, architecture, business rules, security, dataset analysis and OpenAPI.
- `hiring-assessment-backend`: FastAPI implementation, database, importer, background scoring worker, LLM integration and source dataset.
- `hiring-assessment-frontend`: Next.js candidate and HR user interfaces.

## Read order
1. `requirements/product-requirements.md`
2. `architecture/system-architecture.md`
3. `architecture/database-design.md`
4. `business-rules/scoring-and-submission-rules.md`
5. `architecture/security.md`
6. `contracts/openapi.yaml`
7. `integration/frontend-backend.md`
8. `data/dataset-analysis.md`

## Contract governance
The OpenAPI document in `contracts/openapi.yaml` is the interface contract. Frontend and backend must implement the same reviewed version. Tag reviewed snapshots (`specs-v1.0.0`) and record the consumed tag/commit in each implementation repo.

## Workspace
Clone all three repositories as siblings in one Orca workspace. See `ORCA_WORKSPACE.md`.
