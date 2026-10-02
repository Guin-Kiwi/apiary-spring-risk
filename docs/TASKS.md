# TASKS.md

PHASE: 1
STATUS: done

(0=Bootstrap | 1=Specify | 2=Design | 3=Develop | 4=Validate | 5=Deploy)

## Active objective

Choose a defensible late spring frost definition and canton risk metric
from explored data and cited sources before implementing it (owner: the
metric must follow sound data-analytics practice, 2026-10-02).

## Current Use Case

docs/specs/UC-001-EXPLORE-FROST-RISK.md

## Current slice

UC-001 specified: data inventory, exploratory notebook on a station sample,
comparison of frost definitions and risk metrics, ADR-002 proposal.

## Acceptance / validation cues

- See acceptance criteria in `docs/specs/UC-001-EXPLORE-FROST-RISK.md`.
- Evidence: inventory in `docs/PROJECT.md`, executed notebook, ADR-002
  accepted, `bash scripts/test.sh` passing.

## Blockers / assumptions / decisions

- Decision: exploration first; the metric is implemented in UC-002 (owner,
  2026-10-02).
- Decision: no course rubric; standard good practice (CRISP-DM style,
  reproducible, sourced) (owner, 2026-10-02).
- Open: public hive counts per canton; MeteoSwiss product and parameters;
  licence for committing derived outputs.
- Assumption: requirements come from the prototype
  github.com/Guin-Kiwi/apiary-spring-creep; its code is not copied.
- Decision: static Plotly report (ADR-001); MIT licence; solo project,
  commits to `main` allowed when the owner allows it (2026-10-02).

## Evidence links

- `docs/specs/UC-001-EXPLORE-FROST-RISK.md`
- `docs/adr/ADR-001-static-report-delivery.md`

## Next smallest step

DESIGN for UC-001: settle notebook and data folder locations, dependencies
and the station sample.

## Backlog

- UC-002 implement the chosen frost/risk metric as domain rules (TDD)
- Data ingestion from MeteoSwiss and hive-count source
- Static report generation (Plotly HTML + data files)
- Publishing to a static host and embedding in the personal website (DEPLOY)

## Working agreement

- Keep one active use case small enough to deliver vertically.
- Keep one current slice small enough to finish or re-evaluate quickly.
- Prefer reality-based progress over placeholder completeness.
- Update this file when the lifecycle phase, active objective, or current use case changes.
- Record blockers, assumptions, and decisions when they affect delivery.
- Link validation evidence from the relevant project artefact instead of duplicating it here.
- End each work session with the next smallest step clearly stated.
