# Getting the data

The API never moves image bytes. Every series URL it returns points at **public AWS S3 and GCS buckets — no credentials needed** — so the transfer goes directly from cloud storage to you, works the same against the hosted or a local instance, and scales to whole collections. The API's job is to hand you a **manifest** (or the equivalent download commands); you pull the files with a client.

Install the `idc` command with:

```bash
pip install idc-index
```

## Two ways to download

**1. Whole collection** — the simplest path. `cohort/manifest`'s download payload emits this for you when your filter is a single `collection_id`:

```bash
idc download nlst --download-dir ./idc-data
```

**2. From a manifest** — save the `manifest.txt` output (or any SQL result's `series_aws_url` column) and feed it to the CLI:

```bash
# get the manifest
curl -s https://api.imaging.datacommons.cancer.gov/v3/cohort/manifest.txt \
  -H 'content-type: application/json' \
  -d '{"filters": {"terms": {"collection_id": ["nlst"]}}}' > idc_manifest.txt

# download it
idc download-from-manifest idc_manifest.txt --download-dir ./idc-data
```

You can also drive the raw URLs yourself with `s5cmd --no-sign-request` (anonymous access).

### `source`: `aws` vs `gcs`

Manifest and URL requests take a `source` of `aws` (default) or `gcs`. **Both return `s3://` URLs** — GCS is reached through its S3-compatible endpoint rather than a `gs://` URL. This matches how `idc-index` itself works, and is why `idc download-from-manifest` only recognizes `s3://` lines. For `source=gcs`, add `--endpoint-url https://storage.googleapis.com` when driving `s5cmd` directly.

{% hint style="info" %}
IDC is large (100+ TB total). Always check counts and `size_TB` (from `cohort/counts` or `cohort/manifest`) before downloading a broad selection.
{% endhint %}

For more download options and tools, see the [Downloading data](../data/downloading-data/README.md) section.

## Licenses

IDC data is open, but licenses vary per series — typically **CC BY** (commercial use allowed) vs **CC BY-NC** (non-commercial only). Before reusing or redistributing a cohort, check the split for your filter:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/licenses \
  -H 'content-type: application/json' \
  -d '{"terms": {"collection_id": ["nlst"]}}'
```

`licenses` returns the series count and size per license, so you can see at a glance whether the selection is commercial-friendly.

The filter is an ordinary [cohort filter](idc-api-concepts.md#filter-syntax), so you can check the license at any granularity — a whole collection, or a single study or series by its UID:

```bash
# license of one study
curl -s https://api.imaging.datacommons.cancer.gov/v3/licenses \
  -H 'content-type: application/json' \
  -d '{"terms": {"StudyInstanceUID": ["1.3.6.1.4.1.14519.5.2.1.7695.4164.129908397467389975396031099306"]}}'

# license of one series
curl -s https://api.imaging.datacommons.cancer.gov/v3/licenses \
  -H 'content-type: application/json' \
  -d '{"terms": {"SeriesInstanceUID": ["1.3.6.1.4.1.14519.5.2.1.7695.4164.174071765480311650274095134055"]}}'
```

The same applies to `citations`, `cohort/counts`, and the manifest endpoints — `SeriesInstanceUID`, `StudyInstanceUID`, and `PatientID` are all filterable attributes.

### Licenses for a manifest

The manifest itself (`cohort/manifest.txt`, or the `series` / `download` payload of `cohort/manifest`) does **not** carry license information — it's just download URLs. Because a manifest is defined by a cohort filter, the license breakdown for exactly the manifest's contents is the `licenses` response for the **same filter**. Build the manifest and check its licenses with one filter reused across both endpoints.

If you want the license on **each row** of a manifest, build the manifest with SQL instead and select the `license_short_name` column alongside the series URL:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/sql \
  -H 'content-type: application/json' \
  -d '{"sql": "SELECT SeriesInstanceUID, license_short_name, series_aws_url FROM index WHERE collection_id = '"'"'nlst'"'"'", "max_rows": 5}'
```

Every series URL you can `SELECT` this way *is* a manifest, so this gives you a per-series manifest with the license attached.

## Citations

When you publish results using IDC data, include the per-dataset citations **and** acknowledge IDC itself by citing the IDC paper ([Fedorov et al., 10.1148/rg.230180](https://doi.org/10.1148/rg.230180)).

`citations` returns both for your cohort, in your choice of format — `apa`, `bibtex`, `csl-json`, or `turtle`:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/citations \
  -H 'content-type: application/json' \
  -d '{"filters": {"terms": {"collection_id": ["nlst"]}}, "citation_format": "bibtex"}'
```

The response carries the per-dataset citations (from the cohort's source DOIs) and the IDC paper as a separate acknowledgment, so you can drop both straight into your manuscript. See [Publications](../publications.md) for more on citing and acknowledging IDC.
