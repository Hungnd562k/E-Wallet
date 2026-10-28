# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this repository.

## What this is

The documentation hub of the E-Wallet platform. It holds no code. Docs are written in Vietnamese.

- `docs/` on branch `main` is the single source of truth for business rules, requirements and architecture. Start at `docs/README.md` (document map and ID conventions `US-xx`, `BR-xx`, `UC-xx`, `ADR-xx`).
- Exception: each component's API contract lives in that component's repo. For wallet-core it is `E-Wallet-Wallet-Core/api/openapi.yaml` (decision Q-07); docs only link to it. `docs/api/corebank.openapi.yaml` stays here until corebank-sim has its own repo.
- The project is polyrepo (ADR-16). Code repos read these docs; wallet-core's `CLAUDE.md` refers to them as `../E-Wallet/docs/`.
- `docs/chuan-hoa/` is the standardization report (issues `ISS-xx`, questions `Q-xx` with the owner's answers). Check it before changing a source document.
- `progression/` holds planning notes and the Jira backlog draft (Jira project `SCRUM`).

## AI PR review

- Process: `docs/review-process.md`. Shared reviewer rules: `docs/review-policy.md`. Path-to-docs index: `docs/review-context.md`.
- `.github/workflows/ai-review.yml` is a reusable workflow (`workflow_call`) used by every code repo. Changing it changes review for all of them.
- When a code module or a new component repo appears, add it to `docs/review-context.md`.

## Writing rules

- Keep IDs stable; never renumber `BR-xx`, `US-xx`, `UC-xx`, `ADR-xx`. Supersede an ADR with a new one instead of editing its meaning.
- When you change a document, update its status/date line and its history section.
- Link between docs with relative paths and section anchors; the traceability matrix in `docs/chuan-hoa/H-ma-tran-truy-vet.md` and wallet-core's `CLAUDE.md` depend on these paths.
- Lint OpenAPI files with `npx @redocly/cli lint` (config in `redocly.yaml`).
