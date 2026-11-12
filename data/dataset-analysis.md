# Dataset Inspection and Import Rules

Source archive: `business-systems-pack.zip` supplied by the user.
Source README says all data is synthetic and export date is 20 September 2026.

## Files found
- `questions.md`
- `answers/` with 35 `.json` files and 5 matching `.m4a` audio files
- `scoring_sheet.csv`
- `hr_reference_scores.csv`
- `pipeline_log.csv`
- `hr_time_log.csv`
- `inbox_export.csv`
- `README.md`

## Verified counts and structure
- 6 interview questions: Q1–Q6.
- 5 roles: Backend Engineer, Sales Executive, Customer Success Associate, Marketing Coordinator, Finance Analyst.
- 35 answer JSON files.
- 30 answers have a text transcript.
- 5 answers are audio-only (`transcript: null`) and point to a matching audio file:
  A031.m4a, A032.m4a, A033.m4a, A034.m4a, A035.m4a.
- 18 distinct candidate IDs occur in the answer JSON files.
- All answer candidate IDs are present in `pipeline_log.csv`.
- `pipeline_log.csv`: 496 rows; fields:
  `candidate_id,candidate_name,candidate_email,role,applied_on,cv_tier,invited_on,answers_submitted_on,outcome,outcome_on`.
- `scoring_sheet.csv`: 540 rows; fields:
  `answer_ref,candidate_id,role,question_id,scored_on,scorer_1,score_1,scorer_2,score_2`.
- `hr_reference_scores.csv`: 10 rows; fields:
  `answer_id,question_id,score,scored_by`.
- `hr_time_log.csv`: 48 rows; fields:
  `week_starting,task,hours`.
- `inbox_export.csv`: 198 rows; fields:
  `received_at,from_name,from_address,subject,body_preview`.

## Important duplicate in the answer pack
The same candidate/question pair occurs twice:
- `A022`: candidate `C2052`, question `Q3`, submitted `2026-09-18T17:55:00`
- `A023`: candidate `C2052`, question `Q3`, submitted `2026-09-18T18:15:00`

Preserve both. Do not apply a database uniqueness rule that prevents storing both submissions.
Use a logical interview response plus versioned submissions. The later submission is the proposed current/counting version for import; show this clearly in HR UI. Keep the earlier submission visible as superseded.

## Answer JSON shape
Common fields:
- `answer_id`
- `candidate_id`
- `role`
- `question_id`
- `submitted_at`
- `duration_seconds`
- `transcript`

Audio-only records additionally contain:
- `audio_file`
- `transcript` is `null`

Candidate identity/name/email and pipeline metadata come from `pipeline_log.csv`, not the answer JSON.

## Question content
`questions.md` contains six question texts and question-specific 1–4 grading guides. It also states:
- Pass mark: 3; a score of 3 or 4 meets the bar.
- Historical process: each answer was scored by two HR people; candidate advanced when average of both scorers' marks across six answers was 3 or more.

Do not infer that this historical rule should automatically make an AI hiring decision. The app's AI provides suggestions; HR decides.

## Import strategy
1. Import five roles from distinct `pipeline_log.role` values.
2. Import all 496 candidate/application rows from `pipeline_log.csv` idempotently, preserving external candidate IDs and source metadata.
3. Import six questions from `questions.md`, preserving Q1–Q6 and their exact rubric text.
4. Import all 35 answer JSON files as immutable answer submissions.
5. Copy/store the five matching `.m4a` files in private audio storage; persist storage metadata.
6. Create one logical response per candidate/question; mark current submission according to the documented latest-submission policy.
7. Do not automatically score superseded historical submissions. Score current text submissions; audio-only answers remain `needs_a_person` unless real transcription is implemented.
8. Import `hr_reference_scores.csv` into a separate evaluation/reference table or leave it as evaluation-only input. Never feed reference scores to the model.
9. `scoring_sheet.csv`, `hr_time_log.csv`, and `inbox_export.csv` are optional historical/context datasets, not required for the core app. Do not mix them into candidate-facing endpoints.
10. Make imports idempotent and report inserted/updated/skipped counts.

## Evaluation data
`hr_reference_scores.csv` provides ten reference labels. Use only for a separate agreement report after real scoring. It is not the complete ground truth for all 35 answers and must not be used as model output.
