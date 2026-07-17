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

## Citations

When you publish results using IDC data, include the per-dataset citations **and** acknowledge IDC itself by citing the IDC paper ([Fedorov et al., 10.1148/rg.230180](https://doi.org/10.1148/rg.230180)).

`citations` returns both for your cohort, in your choice of format — `apa`, `bibtex`, `csl-json`, or `turtle`:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/citations \
  -H 'content-type: application/json' \
  -d '{"filters": {"terms": {"collection_id": ["nlst"]}}, "citation_format": "bibtex"}'
```

The response carries the per-dataset citations (from the cohort's source DOIs) and the IDC paper as a separate acknowledgment, so you can drop both straight into your manuscript. See [Publications](../publications.md) for more on citing and acknowledging IDC.
