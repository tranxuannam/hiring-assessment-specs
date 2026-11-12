# Product Requirements

Source: `Hiring_Assessment_Brief` supplied in the conversation.
The challenge asks for a small working slice; do not expand scope at the expense of reliability.

## Candidate area — Next.js
- Access interview using a personal link; no candidate account/password.
- Show one question at a time using the six questions in the data pack.
- Accept typed text or an uploaded audio file.
- Confirm immediately when an answer has been received.
- Reloading restores the candidate to the same/current question.

## HR area — Next.js
- HR can sign in. Mock sign-in is allowed only when clearly labelled as mocked.
- HR sees candidates for a role and each answer with its suggested score.
- HR sees the reason for each suggested score.
- HR can override a suggested score; a reason is mandatory.
- HR can see scoring job statuses: `queued`, `scored`, `failed`, `needs_a_person`.
- HR can retry failed scoring jobs.

## Python API and persistent database
- Python JSON API (FastAPI is the selected implementation).
- Backend owns data, business rules, and permissions.
- In-memory-only storage is not acceptable.

## Background scoring
- Each eligible answer is scored in a background job.
- The runtime scorer calls a real language-model API.
- Validate model output before storing it.
- A recorded/cached model response is not a valid runtime implementation.
- Tests may substitute a fake model client.

## Required invariants
- Repeated submission of the same request must not create duplicate scores.
- Candidate must not access grades, other candidates, or HR pages, including by direct API calls.
- Failed scoring is visible to HR and retryable; it must never be displayed as a low score.
- Candidate answer text must not override the scoring instructions.
- Typed answers in the data pack can be loaded and scored.
- Application can be run from README on a clean machine.

## Audio
- Accept and persist audio uploads.
- HR can see that an audio answer arrived.
- Transcription/scoring of audio is optional.

## Optional extras — only after P0
- Compare suggested scores with `hr_reference_scores.csv`.
- Transcribe and score the five audio-only answers (mock transcript is permitted only if labelled).
- Needs-a-person list with reasons.
- Candidate prioritization list.
- Consent screen and microphone check.
- Candidate-side error messages/retry.
- Deployed demo.

## Deliverables
- Private GitHub repository shared with the two addresses specified in the challenge brief.
- Preserve commit history.
- Short README with setup/run/test instructions and one-page architecture note covering data model, API endpoints, answer-to-score lifecycle, and intentionally omitted features.
- Recording of full screen and face throughout the work session; end with walkthrough of both areas handling data-pack answers.
- Send repository and recording links by replying to the invitation email.
