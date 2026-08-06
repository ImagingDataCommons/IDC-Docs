# Licensing and attribution

All data in IDC is publicly available: no registration, no access requests. "Publicly available" is not the same as "unrestricted", though. Every DICOM series in IDC carries a license - most often a Creative Commons license - that defines what you may do with that series and how you must credit the people who produced it.

This page covers which licenses you will encounter in IDC, why the license is a property of the individual _series_ rather than of the collection, how to check the license for any selection of data, and how to attribute what you used.

{% hint style="info" %}
Counts and version numbers on this page are as of IDC data release v24, and will change as new data is added. Nothing on this page is legal advice - when in doubt, read the license text linked below and consult your institution.
{% endhint %}

## Licenses used in IDC

| License                                                                                                        | Commercial use  | Series  | Collections | Share of IDC by size |
| -------------------------------------------------------------------------------------------------------------- | --------------- | ------- | ----------- | -------------------- |
| [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)                                                      | allowed         | 865,935 | 126         | 74.7%                |
| [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/)                                                      | allowed         | 132,303 | 79          | 22.1%                |
| [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)                                                | **not allowed** | 28,783  | 5           | 2.1%                 |
| [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/)                                                | **not allowed** | 5,851   | 4           | 0.8%                 |
| [NLM Terms and Conditions; May 21, 2019](https://www.nlm.nih.gov/databases/download/terms_and_conditions.html) | see terms       | 39      | 1           | 0.3%                 |

Altogether that is 1,032,911 series in 176 collections and 94.7 TB. About 97% of the data by size is under a plain CC BY license that permits commercial reuse; just under 3% is non-commercial. The collections column sums to more than 176 because a single collection can contain series under several licenses - see the next section.

IDC hosts only data that its source has released under a license permitting public redistribution. Where a source collection is split across licenses and part of it is restricted, that part is not in IDC at all: the radiology component of TCGA-GBM is covered by the TCIA limited access license and is absent from IDC, while its digital pathology component is CC BY and is present (see the v9 entry in [data-release-notes.md](data-release-notes.md "mention")).

{% hint style="success" %}
Every license used in IDC - CC BY and CC BY-NC alike - requires **attribution**. See [Attribution and citation](#attribution-and-citation) below for how to produce it.
{% endhint %}

## The license belongs to the series, not to the collection

`license_short_name` is a series-level attribute, exactly like `source_DOI` (see [data-model.md](data-model.md "mention")). Do not assume that the license of a collection tells you the license of everything you selected from it: **39 of the 176 collections in IDC carry more than one license.**

That is because a collection is not licensed as a whole. It is composed of one or more **contributing sources**, each bringing its own DOI, license and citation - `collections_index` records them in its `sources` field, tagged `original_data` or `analysis_result`. A collection carries several licenses whenever its sources do, which happens in two ways.

**A contributed analysis result brings its own license.** An analysis result is a collection in its own right, but its series keep the `collection_id` of the images they analyze - so they sit inside the original collection under different terms. In NSCLC-Radiomics, the original CT images are CC BY-NC 3.0, while the AI-derived annotations added later are CC BY 4.0:

```
license_short_name  series  size_GB
         CC BY 4.0    3661      9.3     <- nnu_net_bpr_annotations (analysis result)
      CC BY-NC 3.0    1265     34.9     <- original CT images
```

**A collection can have several original sources too.** NLST is assembled from two `original_data` sources released under different terms: the CT images from TCIA (`10.7937/tcia.hmq8-j677`, 203,087 series, CC BY 4.0) and the DICOM-converted slide microscopy images (`10.5281/zenodo.12689650`, 1,259 series, CC BY 3.0). No analysis result is involved here - this is simply what the collection is made of.

{% hint style="danger" %}
**The bucket name is not a license label.** It is tempting to read `idc-open-data-cr` as "the non-commercial bucket", but the mapping is not reliable: all 4,433 CC BY-NC 3.0 series of the Phantom FDA collection are stored in `idc-open-data`. Always read `license_short_name` from the metadata - never infer the license from `aws_bucket` or from the URL you downloaded from.
{% endhint %}

{% hint style="warning" %}
**A permissive license on a derived object does not relicense the images it describes.** The `bamf_aimi_annotations` analysis result contributes 897 CC BY 4.0 segmentation series to Duke-Breast-Cancer-MRI, whose 5,161 MR series are CC BY-NC 4.0. The segmentations are of little use without the images, and using the two together means satisfying both licenses - the more restrictive one governs the combination.
{% endhint %}

{% hint style="info" %}
**The license is not inside the DICOM file.** It is metadata that IDC maintains alongside the files, not a DICOM attribute, so it does not travel with the `.dcm` files you download. Record `license_short_name` together with your selection (or keep the manifest that produced it). It is also worth re-checking after a version bump: license assignments are among the mutable metadata IDC may revise between releases - see `mutable_metadata` in [bigquery-tables.md](organization-of-data/bigquery-tables.md "mention").
{% endhint %}

## Checking the license for your selection

### `idc-index`

The `index` table has one row per series, with `license_short_name` among its columns:

```python
from idc_index import IDCClient

client = IDCClient()
client.sql_query("""
SELECT license_short_name, COUNT(*) AS series, ROUND(SUM(series_size_MB)/1024, 1) AS size_GB
FROM index
WHERE collection_id = 'nsclc_radiomics'
GROUP BY license_short_name
ORDER BY series DESC
""")
```

To keep only data you can use commercially, filter on an explicit allowlist rather than excluding the restrictions you happen to know about:

```sql
SELECT SeriesInstanceUID, collection_id, license_short_name, series_aws_url
FROM index
WHERE Modality = 'CT'
  AND license_short_name IN ('CC BY 3.0', 'CC BY 4.0')
```

### BigQuery

BigQuery gives you the license at the granularity of the individual instance, with the full name and the URL of the license text: `license_short_name`, `license_long_name` and `license_url` are columns of the `dicom_all` and `auxiliary_metadata` tables. See [bigquery-tables.md](organization-of-data/bigquery-tables.md "mention").

### REST API

`POST /v3/licenses` returns the license breakdown for any cohort filter:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/licenses \
  -H 'Content-Type: application/json' \
  -d '{"terms": {"collection_id": ["nsclc_radiomics"]}}'
```

```json
{"licenses":[{"license_short_name":"CC BY 4.0","series":3661,"size_TB":0.01},
             {"license_short_name":"CC BY-NC 3.0","series":1265,"size_TB":0.036}]}
```

The response summarizes the series matched by the filter, so you can check the license at any granularity - a whole collection, or a single study or series by its UID. See [getting-data.md](../api/getting-data.md "mention").

### AI assistants

Ask the assistant for the license breakdown of your cohort before you download it; the `get_licenses` tool answers exactly the question above. See [Using IDC with an AI assistant](../agents/README.md).

### IDC Portal

The Explore page lets you filter data by license type, so you can restrict a cohort to commercially reusable data before building a manifest.

## Collections that are not entirely CC BY

As of v24, ten collections contain data under a license other than plain CC BY:

| Collection                     | `collection_id`                  | License                  | Series | Patients |
| ------------------------------ | -------------------------------- | ------------------------ | ------ | -------- |
| Breast-Cancer-Screening-DBT    | `breast_cancer_screening_dbt`    | CC BY-NC 4.0             | 22,032 | 5,060    |
| Duke-Breast-Cancer-MRI         | `duke_breast_cancer_mri`         | CC BY-NC 4.0             | 5,161  | 922      |
| MIDRC-RICORD-1A                | `midrc_ricord_1a`                | CC BY-NC 4.0             | 229    | 110      |
| MIDRC-RICORD-1B                | `midrc_ricord_1b`                | CC BY-NC 4.0             | 120    | 117      |
| MIDRC-RICORD-1C                | `midrc_ricord_1c`                | CC BY-NC 4.0             | 1,241  | 361      |
| NSCLC-Radiomics                | `nsclc_radiomics`                | CC BY-NC 3.0             | 1,265  | 422      |
| NSCLC-Radiomics-Genomics       | `nsclc_radiomics_genomics`       | CC BY-NC 3.0             | 89     | 89       |
| NSCLC-Radiomics-Interobserver1 | `nsclc_radiomics_interobserver1` | CC BY-NC 3.0             | 64     | 22       |
| Phantom FDA                    | `phantom_fda`                    | CC BY-NC 3.0             | 4,433  | 7        |
| NLM-Visible-Human-Project      | `nlm_visible_human_project`      | NLM Terms and Conditions | 39     | 2        |

The counts above are for the restricted series only. Duke-Breast-Cancer-MRI and NSCLC-Radiomics additionally contain CC BY 4.0 analysis results, which is why they also appear among the mixed-license collections.

{% hint style="warning" %}
Treat this table as a snapshot, not as a check you can rely on. Licenses are assigned per series and can be added or revised in any release - query `license_short_name` for your own selection rather than filtering on this list of collection names.
{% endhint %}

## Attribution and citation

Attribution is a condition of every license in IDC, and - like the license itself - it attaches to the series. Each series records the source it came from in `source_DOI`, and that source, not IDC and not the collection, is what you cite. So the citations you owe are determined by the set of `source_DOI` values present in your selection, whatever shape that selection has:

```sql
SELECT source_DOI, license_short_name, COUNT(*) AS series
FROM index
WHERE <your selection>
GROUP BY source_DOI, license_short_name
```

The same selection can span several DOIs and several licenses at once. Even selecting a single patient can do it: in the PROSTATEx example in [data-model.md](data-model.md "mention"), one patient's study spans five DOIs from four contributing groups, under two different licenses.

You do not have to assemble the citations by hand. `POST /v3/citations` (or the `get_citations` tool, if you are working through an AI assistant) resolves the `source_DOI` values of the series matched by your filter into formatted citations. The filter below happens to name a collection, but any cohort filter works - down to a single `SeriesInstanceUID`:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/citations \
  -H 'Content-Type: application/json' \
  -d '{"filters": {"terms": {"collection_id": ["nsclc_radiomics"]}}, "citation_format": "apa"}'
```

This selection covers two DOIs, so two citations come back - one for the original images, one for the AI-derived annotations that were added to the same collection:

> Aerts, H. J. W. L., Wee, L., Rios Velazquez, E., et al. (2019). _Data From NSCLC-Radiomics_ (Version 4) \[Dataset]. The Cancer Imaging Archive. [https://doi.org/10.7937/K9/TCIA.2015.PF0M9REI](https://doi.org/10.7937/K9/TCIA.2015.PF0M9REI)
>
> Deepa Krishnaswamy, Dennis Bontempi, David Clunie, Hugo Aerts, & Andrey Fedorov. (2023). _AI-derived annotations for the NLST and NSCLC-Radiomics computed tomography imaging collections_ \[Dataset]. Zenodo. [https://doi.org/10.5281/ZENODO.7473970](https://doi.org/10.5281/ZENODO.7473970)

Had you selected only the CT images, only the first citation would apply - and the non-commercial restriction along with it. `apa`, `bibtex`, `csl-json` and `turtle` formats are supported.

In addition to the per-source citations, please acknowledge IDC itself by citing the IDC overview publication:

> Fedorov, A., Longabaugh, W. J. R., Pot, D., et al. _National Cancer Institute Imaging Data Commons: Toward Transparency, Reproducibility, and Scalability in Imaging Artificial Intelligence_. RadioGraphics (2023). [https://doi.org/10.1148/rg.230180](https://doi.org/10.1148/rg.230180)

Part 3 of the ["Getting started" tutorial](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting_started/part3_exploring_cohorts.ipynb) walks through licenses, DOIs and attribution hands-on.

## Related pages

* [data-model.md](data-model.md "mention") - why licensing and provenance attach at the series level
* [files-and-metadata.md](organization-of-data/files-and-metadata.md "mention") - buckets and metadata sources
* [data-versioning.md](data-versioning.md "mention") - how data, and its mutable metadata, change between releases
* [getting-data.md](../api/getting-data.md "mention") - licenses and citations via the REST API
