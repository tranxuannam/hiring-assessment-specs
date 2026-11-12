# Task Board

Statuses: TODO / IN_PROGRESS / BLOCKED / REVIEW / DONE.
Only mark DONE with evidence (tests, output, or reviewed diff).

| ID | Task | Owner | Dependencies | Status |
|---|---|---|---|---|
| ARC-01 | Verify supplied data pack and requirements | Architect | None | TODO |
| ARC-02 | Approve architecture and database model | Architect | ARC-01 | TODO |
| ARC-03 | Approve OpenAPI contract | Architect | ARC-02 | TODO |
| DATA-01 | Implement idempotent dataset importer | Backend | ARC-02 | TODO |
| BE-01 | FastAPI/config/database foundation | Backend | ARC-02 | TODO |
| BE-02 | Candidate token/interview/progress APIs | Backend | BE-01, ARC-03 | TODO |
| BE-03 | Answer versioning/idempotency/audio persistence | Backend | BE-01, ARC-03 | TODO |
| BE-04 | HR auth/role/candidate/answer/review APIs | Backend | BE-01, ARC-03 | TODO |
| AI-01 | Real LLM provider adapter + strict schema | Scoring | ARC-03 | TODO |
| AI-02 | Prompt injection handling + scoring tests | Scoring | AI-01 | TODO |
| BE-05 | Persistent worker, claim, retry, recovery | Backend | BE-01, AI-01 | TODO |
| FE-01 | Candidate flow | Frontend | ARC-03 | TODO |
| FE-02 | HR review/dashboard flow | Frontend | ARC-03 | TODO |
| QA-01 | Security/idempotency/worker test plan | QA | ARC-01 | TODO |
| QA-02 | API contract and end-to-end tests | QA | BE-02, BE-03, BE-04, FE-01, FE-02 | TODO |
| DOC-01 | README + one-page architecture note | Architect + Backend | QA-02 | TODO |

## Parallelization
After ARC-01 through ARC-03 are reviewed and committed:
- Backend can start BE-01 / DATA-01.
- Frontend can start FE-01 / FE-02 using the frozen OpenAPI contract and mocks.
- Scoring can start AI-01 / AI-02 in its isolated module.
- QA can prepare QA-01 while implementation proceeds.

## Change control
Any API/database/business-rule change must include:
- Why the change is needed.
- Files/contracts affected.
- Backward compatibility impact.
- Agents that must be notified.
- Tests to update.
Architect approves shared changes.
