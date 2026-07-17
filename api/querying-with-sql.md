# Querying with SQL

Attribute filters (the [cohort surface](idc-api-concepts.md#the-query-surfaces-and-how-they-relate)) cover equality and range conditions on the series-level `index` table. For anything *relational or aggregate* — joins, `GROUP BY`, "X that *also has* Y", per-group counts — or for properties that live only in a specialized index (e.g. the anatomy a segmentation contains), use the SQL surface: `POST /v3/sql` (`run_sql` over MCP).

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/sql \
  -H 'content-type: application/json' \
  -d '{"sql": "SELECT Modality, count(*) n FROM index GROUP BY 1 ORDER BY n DESC", "max_rows": 20}'
```

Ground the schema before you write a query: `GET /v3/tables` lists the available tables and `GET /v3/tables/{table}` returns a table's columns. Don't guess table or column names.

{% hint style="info" %}
**Cohort or SQL?** Use **Cohort** when your selection is attribute filters over series metadata (equality/IN + ranges on the one `index` table) — it's structured and validated. Use **SQL** for anything relational or aggregate, and for properties only a specialized index holds. Anything you can `SELECT series_aws_url FROM index WHERE …` for *is* a manifest, so SQL can also produce download URLs directly.
{% endhint %}

## Limits of the SQL endpoint

`POST /v3/sql` accepts arbitrary SQL, but the data is public and the connection is read-only — you can't break anything. What you *will* run into are the guardrails: only single read-only `SELECT`/`WITH` statements are accepted, and a server row cap and per-query timeout apply. Size-capped results carry a `truncated` flag — for bulk *series*, use the cohort/manifest surface rather than dumping rows through SQL. For the full threat model and hardening details, see the [API security documentation](https://github.com/ImagingDataCommons/IDC-REST-MCP/blob/main/SECURITY.md).

## Tables available to SQL

SQL can reach the main `index` table plus a set of specialized indices. Specialized indices are named `<modality>_index` after the DICOM Modality of the series they describe — if a Modality value is central to your question (SEG, CT, SM, …), check its index. Join them to `index` on `SeriesInstanceUID`.

| Table(s) | Granularity / what it adds |
|---|---|
| `index` | one row per **series** — the main table |
| `collections_index` | one row per collection (curated metadata) |
| `analysis_results_index` | one row per analysis result |
| `version_metadata_index` / `prior_versions_index` | IDC release versions / removed series |
| `seg_index`, `ann_index`, `ann_group_index`, `rtstruct_index` | segmentations / annotations / RT structures: **what was segmented** (`SegmentedPropertyType_CodeMeanings` — note `BodyPartExamined` reflects the source acquisition, not this) and the **reference** to the image series they derive from (`segmented_SeriesInstanceUID` / `referenced_SeriesInstanceUID`) |
| `ct_index`, `mr_index`, `pt_index` | per-modality acquisition parameters (slice thickness, kVp, TE/TR, injected dose…) |
| `sm_index`, `sm_instance_index` | slide-microscopy (pathology) series / instance metadata |
| `contrast_index`, `volume_geometry_index` | contrast agent / 3D volume geometry |
| `clinical_index` | per-collection clinical-table data dictionary |

This is what makes **relational** questions answerable. For example, *"pathology slides that have a segmentation of a specific structure"* — impossible against `index` alone — is a join of `index` (the slides) to `seg_index` (the segmentations) on the segmented image series:

```sql
SELECT i.collection_id, count(DISTINCT i.SeriesInstanceUID) AS slides
FROM index i
JOIN seg_index seg ON seg.segmented_SeriesInstanceUID = i.SeriesInstanceUID
WHERE i.Modality = 'SM'                                    -- slide microscopy (pathology)
  AND list_contains(seg.SegmentedPropertyType_CodeMeanings, 'Nucleus')  -- the segmented structure
GROUP BY 1 ORDER BY slides DESC
```

{% hint style="info" %}
**Array columns:** columns whose schema type is `STRING[]` (e.g. the `*_CodeMeanings` columns above) hold a *list* of values per row — match elements with `list_contains(col, 'value')`, not `=` or `LIKE`. If a query is invalid, the error response carries DuckDB's own message (including its "Did you mean …?" suggestions), so you can fix and retry.
{% endhint %}

{% hint style="warning" %}
**Still BigQuery-only.** A handful of things remain outside these indices: *per-individual-segment* detail (each segment rather than the series-level aggregated code lists in `seg_index`), DICOM SR quantitative/qualitative measurements (radiomics), and private DICOM elements. For those, query the full metadata with [`idc-index`](https://github.com/ImagingDataCommons/idc-index) and [BigQuery](../data/organization-of-data/bigquery-tables.md).
{% endhint %}

## Clinical (non-imaging) data

Many collections ship clinical data — demographics, diagnoses, cancer staging, therapies, labs, outcomes — alongside the images. It comes in two layers:

* **`clinical_index`** — a *data dictionary*: one row per (collection, table, column) with a human-readable `column_label` and an array of coded `values`. Use it to discover *what* clinical attributes a collection has and what their codes mean. It's a normal table — query it with SQL (it joins to `index` on `collection_id`).
* **Per-collection clinical tables** (e.g. `nlst_canc`) — the actual clinical rows. These live under a separate **`clinical` schema** and are queried as `clinical.<table>`. There are ~150 of them, so they are kept out of `GET /v3/tables` and discovered with the [clinical endpoints](endpoint-details.md#worked-examples) instead. Each joins to imaging on **`dicom_patient_id = index.PatientID`** (not `SeriesInstanceUID`). Clinical data is *not harmonized* across collections — table and column names vary, so always discover before querying.

For relational questions — filtering by a clinical attribute, or joining clinical data to imaging — use SQL against `clinical.<table>`. For example, *"NLST patients imaged with CT whose cancer is stage IV (code `400`)"*:

```sql
SELECT count(DISTINCT i.PatientID) AS patients
FROM index i
JOIN clinical.nlst_canc c ON c.dicom_patient_id = i.PatientID
WHERE i.collection_id = 'nlst' AND i.Modality = 'CT'
  AND c.clinical_stag = '400'
```

See [Clinical data](../data/organization-of-data/clinical.md) for more on IDC's clinical data model.
