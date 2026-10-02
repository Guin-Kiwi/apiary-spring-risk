# UC-001-EXPLORE-FROST-RISK

## Goal
Find out, from real data and sources, what should count as a damaging late
spring frost for bees and how to score frost risk per canton. Record the
chosen definition as ADR-002.

## Business value
The frost definition and the risk score are what the whole report rests on.
The prototype's formula (frost days ÷ hives) ranks cantons with more hives as
safer and is biased by station count. Choosing the metric from explored data
and cited sources makes the results defensible and keeps later use cases from
being rebuilt.

## Scope

### In scope
- Data inventory: candidate sources for daily minimum temperatures, station
  metadata (canton, elevation, coordinates) and hive counts per canton, each
  with URL, licence, temporal/spatial coverage, granularity and access method.
- A reproducible exploratory notebook on a small sample of stations (several
  cantons and elevations): data quality (gaps, record length), spring frost
  frequency by month and elevation, and trend over the available decades.
- At least two frost-day definitions and at least two canton risk metrics,
  compared on the sample, each with a cited rationale and its effect on the
  canton ranking (sensitivity).
- ADR-002 proposing the frost definition and risk metric, for the owner to
  accept.

### Out of scope
- Application code in `src/app/` (the chosen metric is implemented in UC-002,
  test-first).
- A full download of all stations, a production pipeline or persistence.
- The published report and hosting (ADR-001, later use cases).
- Forecasts, real-time alerts.

## Actors

Primary: project owner (analyst, decides the metric)
Secondary: coding agent (prepares the inventory, notebook and comparison);
data providers (MeteoSwiss, hive-count source)

## Preconditions

- Phase 0 done; ADR-001 (static report) accepted.
- Network access to the public data sources.
- Python environment with analysis libraries (pandas, Jupyter) installed.

## Trigger
The owner needs a defensible frost-day definition and risk metric before
implementation starts.

## Main flow

1. Identify and document the data sources (inventory) in `docs/PROJECT.md`.
2. Download a small sample (selected stations, full available period) into
   a git-ignored local data folder from the notebook, so it can be reproduced.
3. Assess data quality: missing days, record length per station, station
   moves or changes where documented.
4. Explore spring frost: frequency per month (March–June to check the window),
   per elevation, and per decade.
5. Collect cited evidence for what makes frost harmful to bees (e.g. blossom
   and forage loss, brood chilling, frost after a warm spell) and derive
   candidate frost-day definitions from it.
6. Define candidate risk metrics (e.g. frost hazard only; hazard combined with
   hive counts) and compute them on the sample.
7. Compare the candidates: the canton ranking each produces, sensitivity to
   the threshold, biases (station count, record length, elevation).
8. Write ADR-002 (status Proposed) with the recommendation; the owner accepts
   or changes it.

## Alternate / edge flows

- No public hive count per canton: document the gap, compare hazard-only
  metrics, and list fallback options in ADR-002 (e.g. other published
  statistics), without inventing numbers.
- Cantons without a station, or with only high-elevation stations: record
  how the metric treats them (excluded, nearest station, flagged).
- Short or interrupted records: apply and document a minimum-coverage rule
  per station-year.
- Evidence suggests a different season window than April–June: record it and
  use it in the candidate definitions.

## Errors / failure cases

- Source unavailable or format changed: notebook stops with a clear message;
  inventory notes the problem.
- Licence does not allow republishing the data or derived results: flag it
  before ADR-002; publishing is blocked until resolved.
- Downloaded file fails basic validation (missing columns, impossible
  temperatures): excluded and reported in the notebook.

## Acceptance criteria

- Given the data inventory in `docs/PROJECT.md`
  When the owner reviews it
  Then each source lists URL, licence, coverage, granularity and access
  method, and missing data (e.g. hive counts) is stated explicitly.
- Given a clean checkout and the documented command
  When the exploratory notebook is run top to bottom
  Then it downloads the sample, runs without manual steps, and produces the
  same tables and charts as the committed run.
- Given the notebook's comparison section
  When the owner reads it
  Then at least two frost-day definitions and two risk metrics are compared
  on the sample, each with a cited rationale, its canton ranking and its
  known biases.
- Given ADR-002
  When the owner accepts it
  Then it states the frost-day definition, the risk metric with formula,
  inputs, handling of missing data and the alternatives rejected, and
  `docs/PROJECT.md` links it.
- Given the changes of this use case
  When `bash scripts/test.sh` runs
  Then it passes, and no raw data files are committed.

## Security / trust-boundary notes

- External input: downloaded files from public sources over HTTPS; validate
  columns and value ranges before use.
- No credentials or personal data expected; if a hive-count source contains
  beekeeper names or exact apiary locations, use only aggregates per canton
  and do not commit the raw data.
- Respect source licences and attribution (to be recorded in the inventory).

## Validation plan

### Tests
Unit: none (no application code; existing tests must still pass)
Integration: none
E2E: notebook executes top to bottom from a clean environment (command
documented in `docs/PROJECT.md`)

### Evidence
Data inventory in `docs/PROJECT.md`, executed notebook, ADR-002 accepted by
the owner, `bash scripts/test.sh` output. At VALIDATE, replace this with the
actual results.

## Open questions / assumptions

- Open: public source of hive or apiary counts per canton (prototype used
  placeholder URLs).
- Open: which MeteoSwiss product and parameters to use; assumption, to
  verify: MeteoSwiss open data offers daily minimum air temperature (2 m) and
  ground-level minimum, which may differ for blossom frost.
- Open: whether the notebook's executed outputs (derived tables/charts) may
  be committed under the data licence.
- Assumption: a sample of stations is enough to choose the metric; the full
  dataset is processed in later use cases.
- Assumption: the analysis follows CRISP-DM's data-understanding step; no
  course rubric applies (owner, 2026-10-02).

## Notes for implementation

- Prototype for reference only:
  github.com/Guin-Kiwi/apiary-spring-creep (`etl/transform.py`).
- Notebook location (e.g. `notebooks/`) and a git-ignored data folder
  (`data/` is not ignored yet) to be settled in DESIGN; `notebooks/` is not
  yet listed in `docs/INDEX.json` (review-gated).
- New dependencies (pandas, Jupyter, an HTTP client if needed) are added in
  this use case; keep them out of `src/app/domain` and `application`
  (architecture test).
- ADR-001: the final output is a static report; ADR-002 should name the
  fields the report needs.
