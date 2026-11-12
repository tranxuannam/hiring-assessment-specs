# Shared Specs Repository — Agent Instructions

This repository is the Single Source of Truth for cross-repository requirements.
Read relevant files here before implementing or changing behavior in frontend/backend.

## Ownership
- Product requirements, architecture, business rules, security rules and OpenAPI contract live here.
- Frontend and backend repositories consume this repository; they must not silently fork or redefine these rules.
- Any cross-repository behavior change starts with an update here and a reviewed commit/tag.

## Rules
1. Do not invent requirements, API endpoints, schemas, scoring criteria or role permissions.
2. Distinguish source-derived requirements from implementation proposals.
3. Preserve source dataset identifiers and provenance.
4. Never expose HR reference scores or internal HR notes through candidate-facing APIs.
5. Keep AI scoring advisory; a human HR reviewer owns the final decision.
6. When changing OpenAPI, document compatibility impact and notify both implementation agents.
7. Use semantic commits and tag reviewed contract snapshots, e.g. `specs-v1.0.0`.

## Before finishing
- List changed shared rules/contracts.
- Identify affected repositories and required follow-up work.
- Record unresolved assumptions rather than hiding them.
