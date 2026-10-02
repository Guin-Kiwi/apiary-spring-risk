# Apiary Spring Risk 🐝❄️

How exposed are Swiss bee colonies to late spring frosts?

Frost from April to June can kill brood and damage forage just as colonies
build up. This project combines historical daily minimum temperatures from
MeteoSwiss with cantonal hive counts to:

- detect spring frost events (daily minimum ≤ 0 °C, April–June)
- compute a frost risk score per canton, weighted by hive density
- show how frost frequency has changed over recent decades

The results are published as a **static, interactive report** (Plotly charts
plus downloadable data) that is embedded in or linked from the author's
website. No server is needed to view it
([ADR-001](docs/adr/ADR-001-static-report-delivery.md)).

> **Status:** early development. The project context is defined; the first
> use case (frost events and canton risk score) is next. Data sources and
> their licences will be documented here once confirmed.

## Run it

Requires Python 3.12.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
bash scripts/test.sh
```

In GitHub Codespaces the environment is set up automatically
(`scripts/setup-python.sh`). The command that generates the report will be
added here once it exists.

## Project layout

| Path | Content |
|---|---|
| `src/app/domain/` | Frost and risk rules (pure Python) |
| `src/app/application/` | Use cases: run the pipeline, query risk |
| `src/app/interfaces/` | Report generator entry point |
| `src/app/infrastructure/` | Data sources, storage, chart rendering |
| `tests/` | Unit, integration and end-to-end tests |
| `docs/PROJECT.md` | Purpose, architecture, commands, dependencies |

## How this project was developed (AI-SDLC)

This project follows the AI-Assisted Software Development Life Cycle: small
use cases go through SPECIFY → DESIGN → DEVELOP → VALIDATE → DEPLOY, tests
come first (TDD), and a human approves each step.

- [`CONTRIBUTING.md`](CONTRIBUTING.md) — workflow, branches, reviews
- [`docs/specs/`](docs/specs/) — use-case specifications
- [`docs/adr/`](docs/adr/) — architecture decisions
- [`docs/TASKS.md`](docs/TASKS.md) — current lifecycle state
- [`docs/future/`](docs/future/) — non-binding notes on skill design

## Licence and credits

Project code and documentation: MIT, © 2026 Ayla Allen (`LICENSE` section 2).
The AI-SDLC process files are adapted from the AI-SDLC Template by Andreas
Martin and Sandro Schwander (FHNW), CC BY 4.0 (`LICENSE` section 1). See
[`CITATION.cff`](CITATION.cff) to cite this project.
