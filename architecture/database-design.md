# Database Schema Proposal

Use SQLAlchemy models and migrations or a reproducible schema initialization process.

## roles
- `id` PK
- `name` unique, not null

## candidates
- `id` PK
- `external_id` unique, not null (e.g. C2056)
- `role_id` FK roles
- `name`, `email`
- `applied_on`, `cv_tier`, `invited_on`, `answers_submitted_on`
- `pipeline_outcome`, `outcome_on`
- timestamps

## candidate_access_tokens
- `id` PK
- `candidate_id` FK
- `token_hash` unique, not null
- `created_at`, `expires_at`, `revoked_at`
Store only a cryptographic hash. Raw token is shown once during local demo seed and never logged.

## questions
- `id` PK
- `external_id` unique (Q1..Q6)
- `position` unique
- `question_text`
- `grading_guide`
- `pass_mark` (source says 3)

## interview_responses
One logical candidate/question slot.
- `id` PK
- `candidate_id` FK
- `question_id` FK
- `current_submission_id` nullable FK answer_submissions
- `created_at`, `updated_at`
- unique `(candidate_id, question_id)`

## answer_submissions
Immutable versions; supports the supplied A022/A023 repeated candidate/question pair.
- `id` PK
- `external_id` nullable unique (source answer ID, e.g. A022)
- `response_id` FK interview_responses
- `version_number`
- `idempotency_key` nullable unique within candidate/API scope; enforce appropriate composite uniqueness
- `answer_text` nullable
- `audio_storage_key` nullable
- `audio_original_filename` nullable
- `audio_content_type` nullable
- `audio_size_bytes` nullable
- `submitted_at`
- `duration_seconds` nullable
- `source` (`data_pack` or `candidate_submission`)
- `is_current` boolean
- `superseded_at` nullable
- unique `(response_id, version_number)`

## scoring_jobs
- `id` PK
- `submission_id` FK answer_submissions, unique
- `status` enum/string: queued, scored, failed, needs_a_person; optional internal processing state/lease fields
- `attempt_count`
- `max_attempts`
- `next_attempt_at`
- `claimed_at`, `lease_expires_at`
- `last_error_code`, `last_error_message` (sanitized)
- timestamps

## scores
- `id` PK
- `job_id` FK scoring_jobs unique
- `submission_id` FK answer_submissions unique for one score per submission
- `suggested_score` nullable integer 1..4
- `reason` nullable
- `model_name`, `prompt_version`
- `created_at`

A `needs_a_person` job has no numeric suggested score unless a valid score is separately supplied under an explicitly documented workflow.

## hr_reviews
- `id` PK
- `submission_id` FK answer_submissions
- `reviewer_id` or demo reviewer identifier
- `final_score` integer 1..4
- `reason` non-empty
- `created_at`
- optional `supersedes_review_id` for audit history

Keep AI suggestion and HR decision separate. HR override must never overwrite the original AI score.

## reference_scores (optional evaluation-only)
- `id` PK
- `external_answer_id`
- `question_external_id`
- `reference_score` 1..4
- `scored_by`
Do not expose this table to the candidate API or include it in LLM prompts.

## Key indexes/constraints
- unique candidate external ID
- unique question external ID
- unique logical response `(candidate_id, question_id)`
- unique submission external ID
- unique `(response_id, version_number)`
- unique scoring job per submission
- unique score per submission
- indexes on candidate role, job status/next_attempt_at, response candidate/question

Do not put `UNIQUE(candidate_id, question_id)` on answer_submissions; that would reject A022/A023.
