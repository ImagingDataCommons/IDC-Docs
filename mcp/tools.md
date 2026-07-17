# Tools and resources

The IDC MCP server exposes the same capabilities as the [REST API](../api/README.md), grouped by the [query surfaces](../api/idc-api-concepts.md#the-query-surfaces-and-how-they-relate) described for the API. Tool descriptions are prescriptive about *when* to call each one, and the server ships an `idc://guide` resource with the same conceptual model as the [API Concepts](../api/idc-api-concepts.md) page — so a capable agent can follow the recommended ground-first workflow without extra prompting.

## Tools, by surface

* **Discovery:** `get_idc_version`, `get_stats`, `list_collections`, `get_collection`, `list_analysis_results`, `list_attributes`, `get_attribute_values`
* **Schema (for SQL):** `list_tables`, `get_table_schema`
* **Clinical data:** `list_clinical_tables`, `get_clinical_table_schema`, `get_clinical_table`
* **Cohort / query:** `build_cohort`, `run_sql`
* **Retrieval & side tools:** `get_cohort_urls`, `get_viewer_url`, `get_citations`, `get_licenses`

Each tool maps to the REST endpoint of the same purpose (see the [Endpoint Details](../api/endpoint-details.md) reference). For example, `build_cohort` corresponds to `POST /v3/cohort/manifest`, `run_sql` to `POST /v3/sql`, and `get_cohort_urls` to `POST /v3/cohort/manifest.txt`. `run_sql` is safe to leave in an agent's hands: the data is public, the connection is read-only, and the same guardrails apply as for the REST endpoint (see [Limits of the SQL endpoint](../api/querying-with-sql.md#limits-of-the-sql-endpoint)).

## Resources

* `idc://guide` — the data model and recommended workflow (the same conceptual model as the [API Concepts](../api/idc-api-concepts.md) page).
* `idc://tables` — the tables available to SQL.
* `idc://schema/{table}` — the column schema for a given table.

## Typical agent workflow

Given a natural-language request, a well-behaved agent will:

1. **Ground values** with `list_attributes` and `get_attribute_values` (or **ground the schema** with `list_tables` / `get_table_schema` for a relational question).
2. **Build and size** the cohort with `build_cohort`, or query directly with `run_sql`.
3. **Retrieve** with `get_cohort_urls` — returning `idc` CLI commands and a manifest that pull files directly from public S3/GCS buckets (see [Getting the data](../api/getting-data.md)).
4. **Be a good citizen** — check `get_licenses` (CC BY vs CC BY-NC) and include `get_citations` when publishing.

See [Querying with SQL](../api/querying-with-sql.md) for the tables `run_sql` can reach and the clinical-data layer.
