# PROJECT.md

Complete this document during **BOOTSTRAP**. Keep it concise and specific to
the project created from this template.

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
  - detect spring frost events (daily minimum ≤ 0 °C in April–June)
  - compute a normalised frost risk score per canton, weighted by hive density
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
| `domain` | Frost event rule (spring months, 0 °C threshold), canton risk score; pure Python, no pandas/DB |
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
- Data sources: concrete MeteoSwiss dataset and the cantonal apiary source
  (URL, format, licence) are not yet identified.

## Structure

| Path | Content |
|---|---|
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
| Run locally | `TBD` (report generator entry point, built in DEVELOP) |
| Build/release | `TBD` |

## Dependencies

- Runtime: Python 3.12 (`.python-version`), see ADR-000.
- Manifest: `requirements.txt`: `plotly` (report charts), `pytest`. Further
  libraries (e.g. pandas, an HTTP client) are added with the slice that needs
  them.
- No secrets required so far. Data-source credentials, if any, go in
  environment variables and are documented here by name only.
