# UC-001-EXPLORE-FROST-RISK

## Goal
Find out, from real data and sources, what should count as a damaging late
spring frost for bees and how to score frost risk per canton. First agree the
concepts (how frost reaches colonies through their forage) and the evidence
for each, then record the chosen definition as ADR-002.

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
- **Concept note** (iteration 2): defines hazard, forage timing,
  vulnerability, exposure and impact on colonies; the harm mechanisms (forage
  loss, brood chilling, reduced nectar flow); the measurement levels (2 m air,
  5 cm above grass, soil) and how they relate on frost nights; and lists every
  assumption with its status (verified with source / assumed / open).
- **Forage-plant inventory** (iteration 2): the key spring (March–June) bee
  forage plants in Switzerland, per plant: value for bees (nectar, pollen),
  flowering weeks by region and elevation, frost sensitivity by development
  stage, ability to recover or re-flower, and available MeteoSwiss phenology
  series; each entry cited or marked "no source found".
- **Density-data inventory** (iteration 2): sources for plant or land-use
  density per region (e.g. orchards, rapeseed, meadows, forest) with
  resolution, coverage, licence and access method. Inventory only.
- ADR-002 proposing the frost definition and risk metric, for the owner to
  accept — **paused until the concept note is accepted**, then revised from
  the concept note and inventories.

### Out of scope
- Application code in `src/app/` (the chosen metric is implemented in UC-002,
  test-first).
- A full download of all stations, a production pipeline or persistence.
- The published report and hosting (ADR-001, later use cases).
- Forecasts, real-time alerts.
- Computing density-weighted forage exposure or colony impact (a later use
  case, if the inventories show the data exists).
- Field observations or surveys.

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
8. Write ADR-002 (status Proposed) with the options. *(Done in iteration 1;
   paused after review.)*
9. Write the concept note; the owner reviews and accepts or corrects the
   concepts and assumptions.
10. Build the forage-plant inventory from cited guides (beekeeping forage
    calendars, plant frost-sensitivity guides, MeteoSwiss phenology).
11. Inventory density data sources per region.
12. Revise ADR-002's context and options from items 9–11; the owner decides.

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
- No frost-sensitivity source for a forage plant: mark "no source found";
  do not infer a threshold from another species without labelling it as an
  assumption.
- Forage calendars differ by region or source: record each with its source
  rather than merging them.
- No density data at a useful resolution: state the gap; ADR-002 then
  records how risk is presented without it.

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
- Given the concept note
  When the owner reviews it
  Then every term used in the risk model is defined, each mechanism names
  the definition it would need, and each assumption is labelled verified
  (with source), assumed or open; the owner has accepted it.
- Given the forage-plant inventory
  When the owner reviews it
  Then each plant named as important spring forage by at least one cited
  Swiss or comparable Central European source is listed, with flowering
  timing, stage-dependent frost sensitivity and recovery, each cited or
  marked "no source found".
- Given the density-data inventory
  When the owner reviews it
  Then each candidate source lists resolution, coverage, licence and access
  method, and gaps are stated.
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
Data inventory in `docs/PROJECT.md`, executed notebook, accepted concept
note, forage-plant and density-data inventories, ADR-002 accepted by the
owner, `bash scripts/test.sh` output. At VALIDATE, replace this with the
actual results.

## Open questions / assumptions

- Resolved (DEVELOP): colonies per canton from Agroscope Transfer 528
  (2022, © Agroscope, parsed at run time); flowering dates from MeteoSwiss
  phenology open data (CC BY).
- Scope (DEVELOP, after review): the analysis targets forage loss (frost on
  open blossom); brood chilling is not analysed and is an option in ADR-002.
- Resolved (DESIGN): MeteoSwiss SwissMetNet open data (`ch.meteoschweiz.ogd-smn`)
  offers `tre200dn` (2 m air, daily minimum) and `tre005dn` (5 cm above
  grass, daily minimum); licence CC BY, so derived outputs may be committed
  with attribution ("Source: MeteoSwiss").
- Iteration 2 (owner, 2026-10-02): a single frost threshold is not enough
  to judge harm to colonies; risk depends on which forage plants flower
  where and when, their stage-dependent sensitivity and their density.
  Concepts and evidence come before further assumptions.
- Open: what counts as colonies being "meaningfully affected" (impact
  measure, e.g. length of a forage gap); to be defined in the concept note.
- Open: the threshold question (−1 to −2.2 °C at 2 m; 5 cm vs 2 m for
  low-growing flowers) is decided per plant from the inventory, not globally.
- To verify (concept note): near-ground air (5 cm above grass) is usually
  colder than 2 m air on clear, calm nights, while the soil is warmer;
  dandelion's lower frost impact may come from closing flower heads, its
  position in the grass and continuous re-flowering rather than warmer air.
- Assumption: a sample of stations is enough to choose the metric; the full
  dataset is processed in later use cases.
- Assumption: the analysis follows CRISP-DM's data-understanding step; no
  course rubric applies (owner, 2026-10-02).

## Notes for implementation

- Iteration 2 deliverables are documents; their location (e.g.
  `docs/analysis/`) is settled in DESIGN. No application code.
- Prototype for reference only:
  github.com/Guin-Kiwi/apiary-spring-creep (`etl/transform.py`).

### Design (phase 2, approved by the owner 2026-10-02)

- Components: none in `src/app/`; Clean Architecture layers unchanged.
  Deliverables are `notebooks/uc-001-explore-frost-risk.ipynb`, the data
  inventory in `docs/PROJECT.md` and ADR-002.
- Data access: per-station daily CSVs from
  `https://data.geo.admin.ch/ch.meteoschweiz.ogd-smn/<abbr>/ogd-smn_<abbr>_d_{historical,recent}.csv`
  (`;`-separated, Latin-1, dates `dd.mm.yyyy`); station metadata
  `ogd-smn_meta_stations.csv` (canton, elevation, exposition, coordinates).
  Read with pandas over HTTPS; no extra HTTP client.
- Cache: downloads go to `data/raw/` (add `data/` to `.gitignore`); the
  notebook reuses cached files and can re-download.
- Sample (13 stations): BAS (BL), BER, ABO (BE), GVE (GE), CGI (VD),
  SIO (VS), LUG, MAG (TI), CHU, DAV (GR), SMA, WAE (ZH), STG (SG);
  203–1594 m. Comparison period 1981–latest (`tre005dn` starts 1981);
  trend uses the full `tre200dn` record.
- Known data facts to handle: station counts per canton range 1–26; NW and
  AR have no station; FL (Liechtenstein) is included; BAS is in BL, not BS;
  measurements are not homogenised (state this limitation for trends).
- Dependencies: add `pandas`, `matplotlib`, `ipykernel`, `nbconvert` to
  `requirements.txt`. matplotlib for exploration (images render on GitHub);
  Plotly stays for the report (ADR-001). Keep pandas/plotly out of
  `src/app/domain` and `application` (architecture test).
- Execution: `jupyter nbconvert --to notebook --execute --inplace
  notebooks/uc-001-explore-frost-risk.ipynb`; committed with outputs. Not
  part of `scripts/test.sh` (needs network).
- `docs/INDEX.json`: add `notebooks/` to "project" once the folder exists
  (owner approved; the lifecycle check rejects missing paths).
- ADR-001: the final output is a static report; ADR-002 should name the
  fields the report needs.
