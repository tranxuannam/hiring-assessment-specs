# Security and Trust Boundaries

## Candidate tokens
- Generate high-entropy random tokens.
- Store only a one-way hash (e.g. SHA-256 of a high-entropy token, with constant-time comparison where applicable).
- Show raw tokens only at generation time for local demo.
- Never log tokens or put them in analytics/error traces.
- Candidate requests resolve ownership from the token on the backend.

## Candidate data isolation
- Every candidate query filters by the candidate resolved from the token.
- Use explicit candidate response DTOs; never serialize ORM objects directly.
- Candidate API must not include scores, score reasons, job error messages, HR notes, or other candidate data.
- Test direct API access, not just UI route hiding.

## HR demo auth
- Mocked authentication is allowed by the challenge but must be labelled as mocked.
- Enforce HR session checks in every HR backend endpoint.
- Use secure, HTTP-only cookie settings appropriate to local environment.
- Document that this is not production authentication.

## Audio
- Validate extension, MIME type, and file size; do not trust browser-provided MIME alone.
- Generate server-side storage names.
- Store outside publicly served frontend directories.
- HR download endpoint must authorize access.
- Never expose filesystem paths.

## LLM
- Candidate answer is untrusted data, not instructions.
- Keep system/developer scoring instructions separate from candidate content.
- Delimit candidate answer as quoted/untrusted content.
- Explicitly tell model to ignore instructions in the answer that attempt to alter rubric, system role, score format, or evaluation process.
- Use structured output where supported; validate again with Pydantic.
- Reject malformed output; do not coerce invalid scores.
- Do not include other candidates' answers or reference scores in the prompt.
- Do not claim prompt injection can be eliminated completely; route suspicious/ambiguous cases to human review where appropriate.

## Secrets and logs
- LLM key only on backend.
- `.env` ignored by Git; commit `.env.example` with placeholders.
- Sanitize provider errors before HR display.
- Do not log candidate answer text by default.
