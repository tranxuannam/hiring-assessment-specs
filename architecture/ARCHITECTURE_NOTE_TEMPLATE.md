# Architecture Note

## Data model
Describe roles, candidates, questions, logical interview responses, versioned answer submissions,
scoring jobs, AI scores, HR reviews, and token storage. Explain A022/A023 handling.

## API endpoints
List candidate and HR endpoints from `contracts/openapi.yaml`.
Explain the trust boundary: backend authorization and candidate-safe response DTOs.

## Answer-to-score lifecycle
Describe transactionally persisting submission + job, worker claim, real LLM call,
strict schema validation, persistence, job status, retry, and HR review.

## Deliberately not built
List anything intentionally omitted (production HR identity provider, email, audio transcription,
deployment, etc.). Clearly label mocks.
