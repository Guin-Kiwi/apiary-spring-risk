# PROJECT.md

## Purpose

- Project name: Apiary Spring Risk
- User or organisational problem: late spring frosts (April–June) can harm
  bee colonies. Beekeepers and cantonal bee-health stakeholders lack a simple
  view of which Swiss cantons are most exposed, and how that exposure relates
  to hive density and changes over time.
- Intended users: beekeepers and bee-health / agricultural stakeholders in
  Switzerland (assumption, derived from the reference project).
- In scope:
  - ingest historical daily minimum temperatures (MeteoSwiss) and cantonal
    apiary data (apiaries, hive counts, locations)
  - detect damaging spring frost events for bee forage (definition: ADR-002,
    from UC-001's concept note and forage-plant inventory)
  - assess frost risk to bee forage and colonies per canton
  - show trends over recent decades
  - publish results as a static, interactive report (Plotly HTML + data
    files) embedded in or linked from the owner's personal website
- Out of scope (until specified): real-time weather alerts, forecasts,
  per-hive inspection records, user accounts.
- Reference: prior prototype
  [Guin-Kiwi/apiary-spring-creep](https://github.com/Guin-Kiwi/apiary-spring-creep)
  (ETL in `etl/`, Streamlit dashboard, SQLite via SQLAlchemy). It is used as
  requirements input only; code is re-implemented here through TDD.

## Architecture

Clean Architecture; dependencies point inward:

`domain ← application ← interfaces ← infrastructure`

Planned mapping (from the reference prototype's ETL + dashboard):

| Layer | Responsibility |
|---|---|
| `domain` | Frost event rule and canton risk metric (per ADR-002); pure Python, no pandas/DB |
| `application` | Use cases: run pipeline (extract → transform → load), query risk per canton; defines repository/source ports |
| `interfaces` | Delivery: report generator entry point (CLI) that writes the static report |
| `infrastructure` | MeteoSwiss and apiary-data sources, persistence, Plotly HTML rendering |

Delivery: static report, no server-side Python in production
([ADR-001](adr/ADR-001-static-report-delivery.md)). The pipeline runs offline
or in CI; generated files go to a static host and are embedded (iframe) in or
linked from the owner's site-builder website.

Open decisions (record as ADRs once decided):

- Static host for the report (e.g. GitHub Pages) and publishing automation
  (decided in DEPLOY).
- Persistence: SQLite + SQLAlchemy in the prototype; not yet confirmed.
- Data processing library (pandas in the prototype) — keep it out of `domain`.
- Frost-day definition and canton risk metric: ADR-002 (proposed, from
  UC-001's exploration).

## Data sources

| Data | Source / access | Licence | Coverage, granularity |
|---|---|---|---|
| Daily air temperature: `tre200dn` (2 m minimum), `tre200d0` (2 m mean), `tre005dn` (5 cm above grass minimum) | MeteoSwiss SwissMetNet open data, collection `ch.meteoschweiz.ogd-smn`: per-station CSV `https://data.geo.admin.ch/ch.meteoschweiz.ogd-smn/<abbr>/ogd-smn_<abbr>_d_{historical,recent}.csv` (`;`, Latin-1, `dd.mm.yyyy`) | CC BY 4.0; attribute "Source: MeteoSwiss" | 159 stations; daily; `tre200dn` from 1864 at some stations, `tre005dn` mostly from 1981; `recent` up to yesterday. Raw, not homogenised |
| Station metadata (canton, elevation, exposition, coordinates) | Same collection: `ogd-smn_meta_stations.csv` | CC BY 4.0 | 25 cantons/areas incl. FL; **AR and NW have no station**; 1–26 stations per canton |
| Flowering dates (e.g. cherry `mprua65d`, apple `mmald65d`, dandelion `mtaro65d`, 50 % flowering) | MeteoSwiss phenological observations open data, collection `ch.meteoschweiz.ogd-phenology`: per-station CSV `https://data.geo.admin.ch/ch.meteoschweiz.ogd-phenology/<abbr>/ogd-phenology_<abbr>_y.csv` (dates as `YYYYMMDD`) and `ogd-phenology_meta_stations.csv` | CC BY 4.0; attribute "Source: MeteoSwiss" | 175 stations, 26 species, yearly; ~95–110 stations with cherry/apple/dandelion records from 1981 or earlier still running |
| Beekeepers and colonies per canton | Charrière & Würgler (2024), *Bienenhaltung in der Schweiz und im internationalen Vergleich*, Agroscope Transfer 528, Table 2 (data: AGIS, FOAG), PDF from `ira.agroscope.ch` (publication 56006) | © Agroscope 2024, no open licence: cite; parsed at run time, not redistributed. Republishing derived per-canton figures in the report: to be confirmed | All 26 cantons, 2022 only, 182,300 colonies; no apiary locations or elevations |

## Structure

| Path | Content |
|---|---|
| `notebooks/` | Exploratory analysis (UC-001), committed with outputs |
| `data/` | Local download cache (`data/raw/`), git-ignored |
| `src/app/domain/` | Entities and business rules |
| `src/app/application/` | Use cases and ports |
| `src/app/interfaces/` | Delivery adapters |
| `src/app/infrastructure/` | Data sources, persistence |
| `tests/unit/` | Unit tests, incl. `test_architecture.py` (dependency rule) |
| `tests/integration/` | Adapter/infrastructure tests |
| `tests/e2e/` | End-to-end tests |

## Commands

| Activity | Command |
|---|---|
| Install | `pip install -r requirements.txt` |
| All checks | `bash scripts/test.sh` |
| Unit tests | `python -m pytest tests/unit` |
| Integration tests | `python -m pytest tests/integration` |
| Run exploration notebook (needs network) | `jupyter nbconvert --to notebook --execute --inplace notebooks/uc-001-explore-frost-risk.ipynb` |
| Generate report | not available yet; added with the first report use case |
| Publish | decided in DEPLOY (static host, see ADR-001) |

## Dependencies

- Runtime: Python 3.12 (`.python-version`), see ADR-000.
- Manifest: `requirements.txt`: `plotly` (report charts), `pandas`,
  `matplotlib`, `ipykernel`, `nbconvert`, `pypdf`, `scipy` (exploration notebook),
  `pytest`. Further libraries are added with the slice that needs them.
- No secrets required so far. Data-source credentials, if any, go in
  environment variables and are documented here by name only.
