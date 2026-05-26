# Coding Standards

This document defines the repository conventions used by the Databricks
notebook, documentation, and reviewer artifacts.

## 1. Repository Scope

This repository is notebook-first. The Databricks notebook is the executable
source of truth, while `docs/` and `reports/` support review, handover, and
portfolio presentation.

Keep the root small:

- `notebooks/` for executable Databricks notebooks.
- `docs/` for setup notes, workflow summaries, standards, and handover text.
- `reports/` for exported reviewer artifacts such as HTML and PDF files.
- `src/` only for stable reusable modules when the workflow is productionized.

Do not add local `data/`, `models/`, `outputs/`, or experiment folders unless
the project direction changes away from Databricks-managed storage.

## 2. Artifact Naming

Use stable descriptive names for top-level project artifacts:

- `notebooks/nyc_taxi_databricks_analytics.ipynb`
- `reports/nyc_taxi_databricks_analytics.html`
- `reports/nyc_taxi_databricks_handover_report.pdf`

Use numbered names in `docs/` when ordering matters:

- `docs/0_coding_standards.md`
- `docs/1_project_brief.md`
- `docs/2_databricks_setup.md`
- `docs/3_notebook_summary.md`
- `docs/4_project_handover.md`
- `docs/5_architecture.md`
- `docs/6_data_quality.md`
- `docs/7_model_results.md`

Do not commit Databricks-imported notebooks under `reports/`.

## 3. Notebook Style

Each notebook should include:

- a short purpose statement at the top;
- a configuration section near the top;
- explicit runtime flags for optional downloads, previews, diagnostics, and
  overwrite behavior;
- concise Markdown at section boundaries explaining why the step exists;
- code cells focused on computation, validation, or artifact writing;
- no saved outputs or execution counts when code changes are committed.

Use Markdown for interpretation and code comments for implementation guardrails.
Avoid long narrative blocks inside code cells.

## 4. Python Style

Follow PEP 8 where practical in notebook code:

- group imports near the top;
- use 4 spaces for indentation;
- keep lines under 79 characters where readability allows;
- use f-strings for formatted output;
- add type hints to reusable functions when clear;
- use Google-style docstrings for helper functions.

Keep comments short and decision-oriented. Avoid comments that simply restate
the next line of code.

## 5. Runtime Efficiency

Default notebook runs should focus on the core pipeline:

- keep `RUN_PROFILE_PREVIEWS = False` for normal reruns;
- keep `RUN_MODEL_DIAGNOSTICS = False` unless preparing appendix evidence;
- make downloads idempotent by skipping files that already exist in volumes;
- avoid repeated full-table counts outside required row-retention checks;
- avoid broad displays of large DataFrames;
- use train-only fitting for target encoders, caps, and standardization stats.

Heavy diagnostics are useful during refinement but should be opt-in.

## 6. Data Quality

Quality filters should be explicit and auditable. The notebook should show:

- timestamp validity;
- realistic trip duration, distance, and speed bounds;
- fare and passenger-count constraints;
- valid taxi zone IDs;
- service-specific operating date windows;
- row retention before and after filters.

## 7. Git Hygiene

Do not commit:

- raw taxi data;
- local model checkpoints;
- Databricks working directories;
- notebook checkpoints;
- Python caches;
- ad hoc experiment outputs;
- duplicate `.ipynb` exports in `reports/`.

Commit lightweight source, documentation, and reviewer artifacts only.
