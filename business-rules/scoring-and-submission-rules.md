# Business Rules

## Candidate interview
- Candidate enters using a personal opaque token; no candidate password.
- Candidate can see only their own six-question interview and progress.
- Show one question at a time.
- Candidate sees immediate receipt after successful persistence.
- Current question is derived from server-side response/submission state, not only browser storage.
- Candidate endpoints must not return score, rubric, HR notes, other candidates, or job error internals.

## Submission/version behavior
- A request retry with the same idempotency key and same payload returns the original submission.
- Same idempotency key with a different payload returns 409.
- A deliberate new submission uses a new idempotency key and creates a new immutable version.
- Only one version per candidate/question is marked current/counting.
- Superseded versions remain visible to HR for audit.
- For the supplied A022/A023 pair, preserve both and mark later A023 as current under the import policy.

## Scoring
- Integer score range 1–4, based on that question's exact grading guide.
- The source guide says pass mark 3; this is informational and must not cause automatic hiring decisions.
- AI suggestion and HR final score are separate records/fields.
- `needs_a_person` means no automatic numeric score should be treated as final.
- `failed` is a system/process failure, never score 1.
- Only current/counting submissions are eligible for active scoring in the demo.
- Audio-only answer: persist audio and surface it to HR. Unless transcription is genuinely implemented, route to `needs_a_person` rather than pretending it was scored.

## HR
- HR can see candidates by role, answers, current/superseded versions, score explanation, and job status.
- Mock login must be visibly labelled in UI and README.
- Override requires integer 1–4 and a non-empty reason.
- Store who/when/reason; never overwrite the AI suggestion.
- Retry only failed jobs or explicitly approved retryable states.

## Import
- Import commands must be idempotent.
- Preserve source IDs and original files.
- Do not invent missing transcript content.
- Do not use HR reference scores as LLM results or prompt examples.
