# TASKS.md

PHASE: 0
STATUS: blocked

(0=Bootstrap | 1=Specify | 2=Design | 3=Develop | 4=Validate | 5=Deploy)

## Active objective

Bootstrap the Apiary Spring Risk project: late spring frost risk for Swiss
cantonal apiaries (see `docs/PROJECT.md`). Purpose and structure are recorded;
the delivery framework and template cleanup are still open.

## Current Use Case

None yet (phase 0).

## Current slice

Phase 0: project context in `docs/PROJECT.md`, root profile files committed.

## Acceptance / validation cues

- `docs/PROJECT.md` states purpose, architecture, structure, commands, dependencies.
- `bash scripts/test.sh` passes.
- Framework decided and `requirements.txt` adjusted (open).
- Template cleanup (bootstrap step 5) done with human approval (open).

## Blockers / assumptions / decisions

- Blocked: delivery framework unknown (user, 2026-10-02). Candidates: Streamlit
  (prototype), FastAPI (template default), CLI.
- Blocked: template cleanup (README, docs/future/, CITATION.cff, .zenodo.json,
  CODEOWNERS, LICENSE section 2) deferred by the user.
- Assumption: requirements come from the prototype
  github.com/Guin-Kiwi/apiary-spring-creep; its code is not copied.
- Assumption: intended users are Swiss beekeepers / bee-health stakeholders.
- Open: concrete MeteoSwiss dataset and cantonal apiary data source.
- Decision: solo project; commits to `main` allowed (user, 2026-10-02).

## Evidence links

- `docs/PROJECT.md`
- `bash scripts/test.sh` output (2026-10-02): all tests pass.

## Next smallest step

Decide the delivery framework (or explicitly defer it) and do the template
cleanup; then SPECIFY UC-001: frost event detection + canton risk score as a
pure domain slice (framework-independent).

## Backlog

- UC-001 frost events and canton risk score (domain)
- Data ingestion from MeteoSwiss and apiary source
- Risk view / dashboard
- Frost frequency trend over decades

## Working agreement

- Keep one active use case small enough to deliver vertically.
- Keep one current slice small enough to finish or re-evaluate quickly.
- Prefer reality-based progress over placeholder completeness.
- Update this file when the lifecycle phase, active objective, or current use case changes.
- Record blockers, assumptions, and decisions when they affect delivery.
- Link validation evidence from the relevant project artefact instead of duplicating it here.
- End each work session with the next smallest step clearly stated.
