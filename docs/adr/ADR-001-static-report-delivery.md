# ADR-001-static-report-delivery

## Status
Accepted

## Context
The app should be available on the owner's personal website, which runs on a
site builder / CMS: it can host pages, links and embedded iframes, but cannot
run Python. The underlying data (historical MeteoSwiss temperatures, cantonal
apiary counts) changes rarely, so results only change when the pipeline is
re-run (roughly once a year after spring).

## Alternatives
- Live Streamlit app (as in the apiary-spring-creep prototype), hosted on
  Streamlit Community Cloud or a server and embedded via iframe: rejected. It
  needs a permanently running Python process, and free-tier apps sleep when
  idle, which is a poor fit for rarely changing data.
- FastAPI JSON API plus a page that calls it: rejected. It needs a separate
  Python host and adds an API, CORS and operations work for no user benefit.

## Decision
Deliver the app as a **static report**. A Python pipeline (extract → compute
frost risk → render) runs offline or in CI and generates self-contained HTML
with interactive Plotly charts, plus the underlying data as JSON/CSV. The
generated files are published to a static host and embedded in, or linked
from, the personal website. No server-side Python runs in production.
Approved by the project owner, 2026-10-02.

## Consequences
- Easier: no hosting cost or server to maintain; works with any site builder;
  output is reproducible and versionable.
- Harder: no live interactivity beyond what client-side Plotly offers (filter,
  hover, zoom); new data requires re-running the pipeline and re-publishing.
- Plotly (and pandas, if used) belong to `infrastructure`/`interfaces`; the
  architecture test forbids them in `domain` and `application`.
- Dependencies: the template's FastAPI profile (`fastapi`, `uvicorn`,
  `httpx`) is dropped from `requirements.txt`; `plotly` is added.
- Open, decided in DEPLOY: the static host (e.g. GitHub Pages from this repo)
  and whether publishing is automated by a scheduled GitHub Actions workflow
  (workflows are review-gated).
- `docs/PROJECT.md` is updated to match.

## Supersedes
none
