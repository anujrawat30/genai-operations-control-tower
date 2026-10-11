# GenAI Operations Control Tower

An end-to-end operations analytics portfolio project simulating an AI-training-data services operation. Demonstrates Microsoft SQL Server/T-SQL, Python, Tableau, operational KPI design, SLA monitoring, root-cause analysis, capacity planning, and GenAI-assisted reporting.

> All business entities, targets, and operational records are fictional and intended for portfolio demonstration.

## Business objective

Monitor production, quality, service-level performance, backlog, workforce capacity, and operational risks. Translate raw operational data into management-ready insights and actionable exceptions.

## Technology stack

- Microsoft SQL Server and T-SQL
- Python, pandas, NumPy, SQLAlchemy, pyodbc, statsmodels
- Tableau
- Git and GitHub
- GenAI-assisted analysis with human verification

## Architecture

Raw CSVs → SQL Server staging → typed tables → KPI/reporting layer → Tableau dashboards → exception investigation → action tracking.

## Project progress

- [x] Phase 0 — Project specification and repository setup
- [ ] Phase 1 — Synthetic data generation
- [ ] Phase 2 — Database and data modelling
- [ ] Phase 3 — KPI layer
- [ ] Phase 4 — Exception management
- [ ] Phase 5 — Root-cause drills
- [ ] Phase 6 — Management reports
- [ ] Phase 7 — Capacity and forecasting
- [ ] Phase 8 — Tableau dashboards
- [ ] Phase 9 — Simulated work week
- [ ] Phase 10 — Portfolio packaging and interview preparation

## Repository structure

- `docs/` — project specification, data dictionary, KPI catalog, screenshots
- `data/sample/` — small, sanitized sample CSVs
- `src/data_generator/` — deterministic synthetic data generator
- `src/analysis/` — Python analysis and validation
- `src/forecasting/` — demand and capacity models
- `sql/01_ddl/` — database and table definitions
- `sql/02_load/` — staging and data-loading scripts
- `sql/03_views/` — reporting views
- `sql/04_kpis/` — KPI calculations
- `sql/05_exceptions/` — exception reporting
- `sql/06_rootcause/` — investigation queries
- `reports/` — management reports
- `tableau/` — dashboard documentation and workbooks
- `simulation/` — operational exercises and lessons learned

Detailed commands and validation checks will be added phase by phase.

## What this phase demonstrates

Project scoping, repository documentation, and operational data-model design.

## Data and security policy

Generated raw data, credentials, environment files, database files, and large Tableau extracts must not be committed. Only source code and small synthetic samples should be published.

## License

MIT. See `LICENSE`.
