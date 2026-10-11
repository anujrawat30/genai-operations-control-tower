# Project Specification — GenAI Operations Control Tower

## 1. Business scenario

A fictional AI-data-services vendor delivers annotation, evaluation, and validation work for a fictional technology client. The Operations Data Analyst monitors whether work is completed on time, meets quality targets, and has sufficient staffing.

**Target scale:** 5 projects, approximately 300 workers, 90 days of history, and 200,000–400,000 task records.

All organizations, names, targets, and data are fictional. Targets represent typical industry-practice simulation assumptions, not official company standards.

## 2. Projects and initial targets

| ID | Project | Work type | SLA hours | Tasks/worker/day | Quality target |
|---:|---|---|---:|---:|---:|
| 1 | Atlas | LLM response rating | 24 | 80 | 97% |
| 2 | Beacon | Safety/red-team evaluation | 24 | 45 | 98% |
| 3 | Canvas | Image annotation | 48 | 100 | 96% |
| 4 | Compass | POI/maps validation | 48 | 75 | 97% |
| 5 | Delta | Ad-creative validation | 24 | 65 | 97% |

Targets will be refined in the KPI catalog to ensure consistent denominators, eligibility rules, and reporting grains.

## 3. Operating model

- Hierarchy: Client → Program Manager → Operations/Quality Managers → Operations Data Analyst → Team Leads/SMEs → Annotators/Evaluators.
- Approximately 300 workers across fictional sites in Gurugram, Bengaluru, and Hyderabad.
- Day, evening, and night shifts.
- Work includes annotation, evaluation, human-in-the-loop validation, and quality auditing.
- Simulate weekday/weekend patterns, shift differences, new-hire learning curves, leave, absences, rework, attrition, and imperfect records.

## 4. Initial KPI definitions

- **Completed volume:** distinct eligible tasks marked completed with a valid completion timestamp.
- **SLA compliance:** eligible completed tasks completed within the applicable SLA divided by eligible completed tasks. Report open overdue work separately.
- **Productivity:** eligible completed tasks divided by productive worker-days; compare like-for-like task types.
- **Quality pass rate:** passed audited tasks divided by audited tasks with valid pass/fail results. Report audit coverage separately.
- **Backlog:** eligible received tasks not completed at the reporting cutoff.
- **Rework rate:** eligible completed tasks flagged for rework divided by eligible completed tasks.
- **Utilization:** productive hours divided by the explicitly defined available scheduled hours.
- **Forecast variance:** actual incoming volume minus forecast volume; protect percentage calculations against zero denominators.

Exact eligibility rules, exclusions, thresholds, and SQL implementations will be finalized in Phase 3.

## 5. Data dictionary

All columns below use SQL Server-compatible types.

### projects.csv — one row per project
- project_id INT — primary key
- project_name NVARCHAR(100)
- task_type_id INT
- sla_hours INT
- target_tasks_per_day DECIMAL(8,2)
- quality_target DECIMAL(5,4)

### queues.csv — one row per queue
- queue_id INT — primary key
- project_id INT
- queue_name NVARCHAR(100)
- priority NVARCHAR(20)

### task_types.csv — one row per task type
- task_type_id INT — primary key
- task_type_name NVARCHAR(100)
- target_handle_time_sec INT

### employees.csv — one row per worker
- worker_id INT — primary key
- team_id INT
- team_lead_id INT
- site NVARCHAR(50)
- shift NVARCHAR(20)
- hire_date DATE
- status NVARCHAR(20)
- attrition_date DATE, nullable

### attendance_shifts.csv — one row per worker per shift date
- attendance_id BIGINT — primary key
- worker_id INT
- shift_date DATE
- scheduled_hours DECIMAL(5,2)
- productive_hours DECIMAL(5,2)
- leave_flag BIT
- absence_flag BIT

### tasks.csv — one row per task
- task_id BIGINT — primary key
- project_id INT
- queue_id INT
- batch_id NVARCHAR(50)
- received_at DATETIME2(0)
- assigned_at DATETIME2(0), nullable
- started_at DATETIME2(0), nullable
- completed_at DATETIME2(0), nullable
- worker_id INT, nullable
- handle_time_sec INT, nullable
- status NVARCHAR(20)
- rework_flag BIT
- priority NVARCHAR(20)

### qa_audits.csv — one row per audit
- audit_id BIGINT — primary key
- task_id BIGINT
- auditor_id INT
- audit_date DATETIME2(0)
- result NVARCHAR(20)
- error_type NVARCHAR(100), nullable
- error_severity NVARCHAR(20), nullable

### sla_config.csv — one row per effective SLA rule
- sla_config_id INT — primary key
- project_id INT
- queue_id INT, nullable
- sla_hours INT
- effective_from DATE
- effective_to DATE, nullable

### daily_forecast.csv — one row per project/queue/date
- forecast_id BIGINT — primary key
- forecast_date DATE
- project_id INT
- queue_id INT
- forecast_volume INT
- actual_incoming_volume INT

### issues_log.csv — one row per operational issue
- issue_id INT — primary key
- opened_at DATETIME2(0)
- project_id INT, nullable
- team_id INT, nullable
- issue_type NVARCHAR(50)
- severity NVARCHAR(20)
- description NVARCHAR(500)
- owner NVARCHAR(100)
- status NVARCHAR(20)
- due_date DATE, nullable
- closed_at DATETIME2(0), nullable

## 6. Data generation and quality

- Use Python, pandas, and NumPy with a fixed random seed.
- Generate realistic handle-time distributions, volume patterns, learning curves, rework, attrition, and shift effects.
- Introduce 5–8% messy records across appropriate data-quality dimensions.
- Preserve raw records for investigation; document cleaning decisions and validation counts.
- CSVs use UTF-8, comma delimiters, headers, quoted text where needed, blank fields for nulls, timestamps formatted as `YYYY-MM-DD HH:MM:SS`, and CRLF line endings.
- Commit generator code, not large raw datasets. Each committed sample table must contain no more than 1,000 rows.

## 7. SQL Server conventions

- Target SQL Server 2019 or later.
- Use T-SQL, `TOP`, `TRY_CONVERT`, `COALESCE`, `ISNULL`, `DATEADD`, `DATEDIFF`, `EOMONTH`, `DATEPART`, CTEs, and `STRING_AGG` where appropriate.
- Protect divisions using `NULLIF` and decimal arithmetic.
- Use `GO` where appropriate and make scripts rerunnable.
- Avoid SQL Server 2022-only features.
- Connect from Python using pyodbc and SQLAlchemy with ODBC Driver 17/18. Read credentials from environment variables.

## 8. Planned deliverables

1. Synthetic CSV generator and validation results.
2. SQL Server schema, loading scripts, data model, and reporting layer.
3. KPI catalog and management exception reporting.
4. Interactive root-cause investigation exercises.
5. Daily, weekly, quality, SLA, workforce, and risk reports.
6. Capacity model and demand forecast with accuracy evaluation.
7. Five Tableau dashboards.
8. Five-day operational simulation.

## 9. Security and governance

Use fictional data only. Never commit passwords, tokens, personal data, database backups, or large generated datasets. Validate GenAI-generated explanations against the underlying metrics before presenting them as operational facts.
