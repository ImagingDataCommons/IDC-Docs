# Endpoint reference

This page is the endpoint reference for the IDC REST API, with a worked `curl` example for each surface. Every endpoint is also documented interactively at the [Swagger UI](https://api.imaging.datacommons.cancer.gov/v3/docs), which shows a filled-in request/response for each route and lets you execute it in the browser.

All endpoints are under the base URL `https://api.imaging.datacommons.cancer.gov` with the `/v3` prefix. The examples below use `localhost:8000` only where noted; against the hosted service, substitute the base URL above.

## All endpoints

| Method & path | Purpose |
|---|---|
| `GET /v3/version` | IDC data release served (e.g. `v24`) + pinned index version, and this server's own software version (`api_version`, plus `build` if the deploy stamped one) |
| `GET /v3/stats` | Headline totals (collections, patients, studies, series, `size_TB`) |
| `GET /v3/collections` | List collections (datasets) |
| `GET /v3/collections/{id}` | Collection detail: counts, modalities, license breakdown |
| `GET /v3/analysis_results` | Derived datasets (segmentations / annotations) |
| `GET /v3/attributes` | Filterable attributes (name, type, term/range, categorical) |
| `GET /v3/attributes/{attr}/values?limit=` | Distinct values + counts for an attribute, plus a `note` caveat when one applies (e.g. `BodyPartExamined` ≠ segmented anatomy) |
| `GET /v3/tables` | Tables available to SQL |
| `GET /v3/tables/{table}` | Column schema for a table |
| `GET /v3/clinical/tables?collection_id=` | Per-collection clinical tables (optionally one collection) |
| `GET /v3/clinical/tables/{table}` | Clinical table columns + human-readable labels |
| `GET /v3/clinical/tables/{table}/rows?max_rows=` | Clinical table rows (capped) |
| `POST /v3/cohort/counts` | Distinct counts for a filter (cheap) |
| `POST /v3/cohort/manifest` | Counts + a page of series + download payload |
| `POST /v3/cohort/manifest.txt` | Full manifest as `text/plain` (`s3://`; `source=gcs` reaches GCS's S3-compatible endpoint) |
| `POST /v3/sql` | Guarded read-only SQL (DuckDB) |
| `GET /v3/viewer-url` | OHIF / Slim viewer link for a study or series |
| `POST /v3/citations` | Citations for a cohort |
| `POST /v3/licenses` | License breakdown for a cohort |

## Worked examples

**Read-only lookups (GET)** — no body, just the URL:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/version                 # data release + this server's build
curl -s https://api.imaging.datacommons.cancer.gov/v3/stats                   # headline totals
curl -s https://api.imaging.datacommons.cancer.gov/v3/collections             # list datasets
curl -s https://api.imaging.datacommons.cancer.gov/v3/collections/nlst        # one collection's detail
curl -s https://api.imaging.datacommons.cancer.gov/v3/analysis_results        # derived datasets
curl -s https://api.imaging.datacommons.cancer.gov/v3/attributes              # filterable attributes
curl -s https://api.imaging.datacommons.cancer.gov/v3/tables                  # tables available to SQL
curl -s https://api.imaging.datacommons.cancer.gov/v3/tables/index            # one table's column schema
```

**Discover valid values before filtering:**

```bash
curl -s 'https://api.imaging.datacommons.cancer.gov/v3/attributes/Modality/values?limit=10'
```

**Cheap size check** — the `counts` body is the filter object directly:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/cohort/counts \
  -H 'content-type: application/json' \
  -d '{"terms": {"Modality": ["MR"], "BodyPartExamined": ["BREAST"]}}'
```

**Build a cohort** — `manifest` wraps the filter in a request with paging:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/cohort/manifest \
  -H 'content-type: application/json' \
  -d '{"filters": {"terms": {"Modality": ["MR"], "BodyPartExamined": ["BREAST"]}},
       "page": 0, "page_size": 3}'
```

**Get the full manifest as plain text** (for `idc download-from-manifest` / `s5cmd`):

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/cohort/manifest.txt \
  -H 'content-type: application/json' \
  -d '{"filters": {"terms": {"collection_id": ["nlst"]}}, "source": "gcs"}'
```

**Custom query via SQL** (anything the structured filters can't express — see [Querying with SQL](querying-with-sql.md)):

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/sql \
  -H 'content-type: application/json' \
  -d '{"sql": "SELECT Modality, count(*) n FROM index GROUP BY 1 ORDER BY n DESC", "max_rows": 20}'
```

**License check** — like `counts`, the body is the filter object directly:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/licenses \
  -H 'content-type: application/json' \
  -d '{"terms": {"collection_id": ["nlst"]}}'
```

**Citations for a cohort** — body wraps the filter, like `manifest`:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/citations \
  -H 'content-type: application/json' \
  -d '{"filters": {"terms": {"collection_id": ["nlst"]}}, "citation_format": "apa"}'
```

**Viewer link** for a study (or pass `series_instance_uid=`):

```bash
curl -s 'https://api.imaging.datacommons.cancer.gov/v3/viewer-url?study_instance_uid=1.3.6.1.4.1.14519.5.2.1.7695.4164.129908397467389975396031099306'
```

**Clinical data** — discover and read the per-collection clinical tables:

```bash
curl -s 'https://api.imaging.datacommons.cancer.gov/v3/clinical/tables?collection_id=nlst'  # clinical tables for a collection
curl -s https://api.imaging.datacommons.cancer.gov/v3/clinical/tables/nlst_canc              # clinical table columns + labels
curl -s 'https://api.imaging.datacommons.cancer.gov/v3/clinical/tables/nlst_canc/rows?max_rows=100'  # clinical rows (capped)
```

For filtering by, or joining on, clinical attributes, use SQL against `clinical.<table>` — see [Querying with SQL](querying-with-sql.md).
