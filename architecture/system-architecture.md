# Architecture Decision Record

## Selected architecture
- One Next.js application with separate candidate and HR route groups.
- FastAPI backend; backend is the only authority for data, business rules, and permissions.
- SQLite + SQLAlchemy for the take-home implementation.
- Persistent database-backed worker process for scoring jobs. Avoid adding Redis/Celery unless time and reliability justify it.
- LLM provider adapter behind a `ScoringProvider` interface.
- Audio files stored on local disk outside public static paths for the demo; persist relative storage key and metadata in DB.

## High-level flow
1. Candidate opens a personal token link.
2. Backend hashes/validates the token and resolves the candidate.
3. Candidate loads interview questions/progress.
4. Candidate submits a text answer or audio file.
5. Backend persists a submission and a scoring job transactionally.
6. Worker claims a queued job, loads the question and rubric, and calls a real LLM.
7. Backend validates structured output and stores either a score or a human-review state.
8. HR reads answers, AI suggestions, reasons, and job states.
9. HR may override a suggestion with a mandatory reason.

## Why a submission/version model is required
The supplied answer pack contains a repeated candidate/question pair:
- candidate `C2052`, question `Q3`: `A022` and `A023`.
Both source records must be preserved. Therefore, do not impose a simple unique constraint on `(candidate_id, question_id)` in the raw answer/submission table.

Use:
- `interview_responses`: one logical slot per candidate and question.
- `answer_submissions`: immutable submitted versions linked to a response.
- `interview_responses.current_submission_id`: the version currently counted.
- `scoring_jobs`: one job per submission, but only the current submission is eligible for the active score.
- HR can see previous versions and which one currently counts.

For imported historical duplicates, use deterministic import policy: mark the later `submitted_at` submission as current, preserve earlier submission as superseded, and record this policy in import metadata. The Architect may adjust this after reviewing the source pair, but must not delete either source record.

For live requests:
- Same `Idempotency-Key` + same candidate/response/payload returns the original submission.
- Same key with a different payload returns `409 IDEMPOTENCY_KEY_REUSED`.
- A deliberate new submission uses a new key and creates a new version; mark prior version superseded and ensure only current version is presented as counting.
- Do not create two active scoring jobs for the same submission.

## Background worker
- Claim jobs atomically (transaction + conditional update or equivalent).
- Use bounded retries with exponential backoff for transient provider errors.
- Store attempt count and sanitized error details.
- On exhausted/non-retryable failure set `failed`.
- On model response requiring human review set `needs_a_person`, with no numeric score.
- A worker restart must not lose queued jobs.
- Do not leave jobs stuck indefinitely; document stale-claim recovery if using a processing lease.

## Scope decisions
P0:
- Candidate flow, HR flow, text scoring with real LLM, audio upload/storage, persistent jobs, retry, authorization, idempotency, data import, README.
P1:
- Agreement report against the ten reference scores.
P2:
- Audio transcription/scoring, candidate ranking, deployment, production-grade identity provider/email.

## Non-goals
- Do not build a production HR identity system; clearly label demo auth.
- Do not implement email delivery unless time remains.
- Do not claim audio transcription exists if only file storage is implemented.
- Do not auto-reject/advance candidates based solely on model score. HR remains decision-maker.
