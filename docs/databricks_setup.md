# Databricks Setup Notes

## Workspace

- Recommended runtime: Databricks Runtime with Spark, Delta Lake, and Python support.
- Development cluster: single-node or small autoscaling cluster, depending on data volume and workspace limits.
- Session timezone: the notebook sets Spark timezone to `America/New_York` so day-of-week and hour features align with NYC taxi trips.

## Storage

The notebook uses Unity Catalog volume paths:

```text
/Volumes/workspace/bde/assignment2/green
/Volumes/workspace/bde/assignment2/yellow
```

Confirm that the catalog, schema, and volume exist before running the notebook. If your workspace uses different Unity Catalog names, update the notebook variables in the setup section.

## Git Workflow

- Clone this repository through Databricks Repos or work locally and push to GitHub.
- Keep assignment artifacts in stable folders:
  - `notebooks/` for `.ipynb`
  - `reports/` for `.html` and `.pdf`
  - `docs/` for setup and handover notes
- Commit notebook, report, and documentation changes separately when practical.

## Notebook Standards

- Keep markdown headers aligned with the assignment parts.
- Use parameter variables for catalog, schema, volume, and file paths.
- Avoid hard-coded local-only paths in committed notebooks.
- Clear all secrets, tokens, and private credentials before committing.
- Re-export the HTML report after notebook logic or output changes.
