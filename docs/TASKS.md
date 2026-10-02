# TASKS.md

PHASE: 2
STATUS: done

(0=Bootstrap | 1=Specify | 2=Design | 3=Develop | 4=Validate | 5=Deploy)

## Active objective

Choose a defensible late spring frost definition and canton risk metric
from explored data and cited sources before implementing it (owner: the
metric must follow sound data-analytics practice, 2026-10-02).

## Current Use Case

docs/specs/UC-001-EXPLORE-FROST-RISK.md

## Current slice

UC-001 designed: no `src/app/` components; notebook + `data/raw/` cache,
13-station sample, matplotlib/pandas/nbconvert (see UC-001 "Design").

## Acceptance / validation cues

- See acceptance criteria in `docs/specs/UC-001-EXPLORE-FROST-RISK.md`.
- Evidence: inventory in `docs/PROJECT.md`, executed notebook, ADR-002
  accepted, `bash scripts/test.sh` passing.

## Blockers / assumptions / decisions

- Decision: exploration first; the metric is implemented in UC-002 (owner,
  2026-10-02).
- Decision: no course rubric; standard good practice (CRISP-DM style,
  reproducible, sourced) (owner, 2026-10-02).
- Open: public hive counts per canton.
- Decision (DESIGN): MeteoSwiss SwissMetNet open data, `tre200dn` and
  `tre005dn`, CC BY; matplotlib for exploration; 13-station sample;
  `notebooks/` to be added to `docs/INDEX.json` (owner, 2026-10-02).
- Assumption: requirements come from the prototype
  github.com/Guin-Kiwi/apiary-spring-creep; its code is not copied.
- Decision: static Plotly report (ADR-001); MIT licence; solo project,
  commits to `main` allowed when the owner allows it (2026-10-02).

## Evidence links

- `docs/specs/UC-001-EXPLORE-FROST-RISK.md`
- `docs/adr/ADR-001-static-report-delivery.md`

## Next smallest step

DEVELOP for UC-001: add dependencies and `data/` ignore, build the notebook
(download sample, data quality), then the inventory.

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
