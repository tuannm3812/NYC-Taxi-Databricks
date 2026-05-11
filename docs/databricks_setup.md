# Databricks Setup Notes

## Runtime

- Use a Databricks Runtime that supports Spark, Delta Lake, and Python.
- A single-node cluster is sufficient for development and review; use autoscaling for larger runs.
- The notebook sets Spark timezone to `America/New_York` so hour and weekday features align with NYC taxi operations.

## Storage Layout

The notebook uses Unity Catalog volume paths for raw green and yellow taxi files:

```text
/Volumes/workspace/bde/nyc_taxi/green
/Volumes/workspace/bde/nyc_taxi/yellow
```

If your workspace uses different catalog, schema, or volume names, update the setup variables at the top of the notebook before execution.

## Execution Workflow

1. Clone the repository into Databricks Repos.
2. Open `notebooks/nyc_taxi_databricks_analytics.ipynb`.
3. Attach a compatible cluster.
4. Confirm data access permissions and Unity Catalog paths.
5. Run all cells in order.
6. Export a refreshed HTML report after any material logic or output change.

## Engineering Standards

- Keep configuration values grouped in the setup section.
- Avoid local-only paths in committed notebooks.
- Do not commit secrets, tokens, or private credentials.
- Keep markdown narratives short, decision-oriented, and tied to the outputs.
- Move repeated logic into `src/` modules as the workflow matures.
