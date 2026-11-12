# Orca Multi-Repository Workspace

Expected local layout:

```text
workspace/
├── hiring-assessment-specs/
├── hiring-assessment-backend/
└── hiring-assessment-frontend/
```

Open the parent `workspace/` directory in Orca so agents can read the shared specs and their assigned implementation repository.

## Agent boundaries
- Architect: primarily edits `hiring-assessment-specs`.
- Backend: reads specs; edits only `hiring-assessment-backend`.
- Frontend: reads specs; edits only `hiring-assessment-frontend`.
- QA/integration: may inspect all three; changes tests in the owning implementation repo unless explicitly assigned otherwise.

`AGENTS.md` is local guidance, not a magical cross-repository loader. Ensure all three sibling repositories are cloned and visible to the agent's workspace. If Orca is configured to isolate agents to a single repository, attach/mount the specs repository as readable context.

## Contract update workflow
1. Propose change in specs first.
2. Review and commit the updated OpenAPI/business rules.
3. Tag or record the exact specs commit.
4. Backend implements and adds/updates contract tests.
5. Frontend regenerates API types and updates API usage.
6. Integration agent verifies both implementations against the same contract.
