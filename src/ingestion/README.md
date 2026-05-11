# Ingestion

Reusable ingestion utilities should live here as the notebook workflow is productionized.

Recommended responsibilities:

- Resolve Unity Catalog volume paths.
- Download or register raw taxi data files.
- Validate file presence, size, and expected format.
- Expose ingestion functions that can be reused by Databricks jobs.
