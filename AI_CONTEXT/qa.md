# QA / Integration Task

Own test plans and `AI_CONTEXT/reports/`.

Prepare and run:
1. Candidate token isolation and direct API privacy tests.
2. HR endpoint authorization tests.
3. Same idempotency key replay test.
4. Same key/different payload conflict test.
5. New submission creates a version and only one current version.
6. Dataset import idempotency; counts and A022/A023 behavior.
7. LLM schema validation including bool/float/out-of-range.
8. Real provider call in runtime configuration; fake client only in tests.
9. Provider failure => failed, no score; retry => queued.
10. needs_a_person has no misleading numeric score.
11. HR override requires reason and preserves AI suggestion.
12. Audio upload persistence and HR-only access.
13. README clean setup.

Report actual commands and outputs. Do not mark tests passed if not executed.
