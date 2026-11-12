# Integration and Context Synchronization Rules

## Contract-first
- `../hiring-assessment-specs/contracts/openapi.yaml` is the interface contract.
- Frontend must not invent request/response fields.
- Backend must not silently change the contract.
- Scoring module exposes a typed Python interface; Backend owns worker orchestration.
- Contract version in this starter is 1.0.0.

## Worktree rules
- Start each agent from the same approved baseline commit.
- Use separate branches/worktrees for frontend, backend, and scoring.
- Uncommitted changes in one worktree are not assumed visible in another.
- Share work through commits and reviewed merges.
- Do not merge generated code without reading the diff.

## Integration sequence
1. Merge scoring module after interface and tests are reviewed.
2. Merge backend and run backend tests.
3. Merge frontend and run typecheck/build/tests.
4. Run contract tests against the integrated API.
5. Run end-to-end candidate and HR journeys.
6. Run security/idempotency/retry tests.
7. Update QA report and task board.

## Checkpoint format
Each agent writes `AI_CONTEXT/reports/<TASK-ID>-checkpoint.md`:
- Task ID / agent / branch / contract version
- Status
- Completed
- Files changed
- Decisions
- Tests run and exact result
- Blockers
- Next steps
- Dependencies for other agents

## Context loss / compaction
When resuming:
1. Read global `AGENTS.md`.
2. Read current task file.
3. Read latest checkpoint.
4. Inspect source and Git status/diff.
5. Verify current contract version.
6. Continue from last verified state; do not repeat blindly.

## Definition of integrated
A feature is integrated only when actual FE requests work against actual BE responses. Mock-only flows are not integrated.
