# Evidence and reproduction

This page backs up the claims in [Is IDC data FAIR?](README.md) For each principle it gives the numbers behind the claims, where they come from, and the queries that reproduce them. Everything here runs on a laptop, without a login or a cloud account.

{% hint style="info" %}
All numbers are for IDC data release v25, obtained on 5 November 2026. External records - DataCite, the registries and the DICOM standard - were read on the same day. The same queries run against a later release will give different numbers.
{% endhint %}

## Sources <a href="#sources" id="sources"></a>

| Source                                                                                                                                  | Used for                                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [`idc-index`](https://github.com/ImagingDataCommons/idc-index) 0.13.4, which installs `idc-index-data` 25.0.5, the metadata of IDC v25  | Nearly all counts: identifiers, formats, references, licenses, provenance, version history  |
| The public parquet export of IDC's BigQuery tables, `s3://idc-open-metadata/bigquery_export/idc_v25/`                                   | Segment codes, which `idc-index` does not carry; the list of releases                        |
| The [DataCite REST API](https://support.datacite.org/docs/api)                                                                          | Registration state and license of the 242 dataset DOIs                                       |
| DICOM standard, edition 2026d: [PS3.16 Annex L](https://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_L.html)        | Body Part Examined defined terms                                                             |
| The [re3data](https://www.re3data.org/repository/r3d100014074) and [FAIRsharing](https://fairsharing.org/10.25504/FAIRsharing.0b5a1d) records of IDC | Registry listings                                                                 |

### Where the metadata comes from <a href="#where-the-metadata-comes-from" id="where-the-metadata-comes-from"></a>

IDC's metadata starts in the DICOM files. IDC loads the files into its DICOM stores, exports the attributes of every file to BigQuery, and adds its own metadata there: collections, licenses, provenance and versions. The BigQuery tables are also exported as parquet to the public bucket above. `idc-index` packages subsets of these tables, chosen for convenience, as parquet files that it installs or downloads on first use; the SQL that builds each subset is public in the [`idc-index-data`](https://github.com/ImagingDataCommons/idc-index-data) repository. The REST API and the MCP server query the same subsets.

This page uses `idc-index` only because it runs locally without a login. Each section names the DICOM attribute behind the columns it queries, so the same results can be obtained from BigQuery or from the files themselves.

## Setup <a href="#setup" id="setup"></a>

```bash
pip install idc-index==0.13.4
```

```python
from idc_index import IDCClient

client = IDCClient()
print(client.get_idc_version())  # v25

# Specialized tables are downloaded on first use
for name in ["seg_index", "rtstruct_index", "ann_index", "prior_versions_index",
             "clinical_index", "collections_index", "analysis_results_index"]:
    client.fetch_index(name)
```

`client.sql_query(sql)` runs a DuckDB SQL query over these tables and returns a pandas DataFrame. Every SQL query below is meant to be passed to it, unless it says otherwise.

## Counting conventions <a href="#counting-conventions" id="counting-conventions"></a>

* **Series** are the unit of counting. Each row of the `index` table is one series, and percentages are of series unless stated otherwise.
* **Patients** are counted as distinct pairs of collection and `PatientID`: 86,451. Counting `PatientID` values alone gives 86,362, because some values occur in more than one collection.
* **Size.** `series_size_MB` is in megabytes of 10<sup>6</sup> bytes. The total, 99.94 TB, is in terabytes of 10<sup>12</sup> bytes, and equals 90.9 TiB in binary units.
* **Segments** are counted once per segmentation instance and segment number. The parquet `segmentations` table repeats some rows, so counting its rows gives a slightly higher number.
* **Segmentation series.** `seg_index` covers the 193,283 series that are DICOM segmentation objects: 193,154 Segmentation and 129 Label Map Segmentation series. The `index` table has 12 more series with the modality `SEG`, which are in fact CT images (see [I1](#i1)).

## Findable

### F1, F3. Identifiers <a href="#f1" id="f1"></a>

```sql
SELECT
  COUNT(DISTINCT collection_id)                     AS collections,
  COUNT(DISTINCT analysis_result_id)                AS analysis_results,
  COUNT(DISTINCT collection_id || '/' || PatientID) AS patients,
  COUNT(DISTINCT PatientID)                         AS distinct_patient_ids,
  COUNT(DISTINCT StudyInstanceUID)                  AS studies,
  COUNT(*)                                          AS series,
  COUNT(DISTINCT SeriesInstanceUID)                 AS distinct_series_uids,
  COUNT(DISTINCT crdc_series_uuid)                  AS distinct_series_uuids,
  SUM(instanceCount)                                AS instances,
  ROUND(SUM(series_size_MB) / 1e6, 2)               AS size_TB,
  ROUND(SUM(series_size_MB) * 1e6 / 2^40, 2)        AS size_TiB,
  COUNT(DISTINCT lower(source_DOI))                 AS dataset_dois,
  COUNT(series_aws_url)                             AS series_with_download_url
FROM index
```

| Column                     |      v25 |
| -------------------------- | -------: |
| `collections`              |      179 |
| `analysis_results`         |       26 |
| `patients`                 |   86,451 |
| `distinct_patient_ids`     |   86,362 |
| `studies`                  |  168,507 |
| `series`                   | 1,044,191 |
| `distinct_series_uids`     | 1,044,191 |
| `distinct_series_uuids`    | 1,044,191 |
| `instances`                | 58,784,975 |
| `size_TB`                  |    99.94 |
| `size_TiB`                 |    90.90 |
| `dataset_dois`             |      242 |
| `series_with_download_url` | 1,044,191 |

The number of series equals the number of distinct Series Instance UIDs and of distinct IDC UUIDs, so both identifiers are unique within the release (F1). Every row carries its identifiers and a download URL (F3, [A1](#a1)).

### F2. Rich metadata <a href="#f2" id="f2"></a>

`index` has 31 columns. The modality-specific tables `ct_index`, `mr_index` and `pt_index` have 21, 22 and 21 columns, one of which is the series identifier; `DESCRIBE ct_index` lists them.

Body Part Examined (0018,0015), the `BodyPartExamined` column of `index`, missing or empty, by modality:

```sql
SELECT Modality,
       COUNT(*) AS series,
       COUNT(*) FILTER (WHERE BodyPartExamined IS NULL OR trim(BodyPartExamined) = '') AS missing,
       ROUND(100.0 * missing / series, 1) AS percent_missing
FROM index
WHERE Modality IN ('CT', 'MR', 'PT', 'US', 'MG', 'CR', 'DX', 'NM', 'XA')
GROUP BY Modality
ORDER BY series DESC
```

| Modality | Series  | Missing | Missing, % |
| -------- | ------: | ------: | ---------: |
| CT       | 270,830 |  31,573 |       11.7 |
| MR       | 128,143 |  42,417 |       33.1 |
| MG       |  48,128 |       0 |        0.0 |
| CR       |  12,534 |       0 |        0.0 |
| PT       |   5,929 |   2,668 |       45.0 |
| DX       |   3,712 |       2 |        0.1 |
| US       |   2,249 |   2,115 |       94.0 |
| NM       |     100 |      70 |       70.0 |
| XA       |      41 |      41 |      100.0 |

### F4. Registries <a href="#f4" id="f4"></a>

As read on 5 November 2026:

* **re3data** ([r3d100014074](https://www.re3data.org/repository/r3d100014074)) lists three interfaces: the REST API (`https://api.imaging.datacommons.cancer.gov/v3`), the DICOMweb proxy and the MCP server. DICOM appears only as a keyword: the record's metadata standard and persistent identifier system fields are empty.
* **FAIRsharing** ([10.25504/FAIRsharing.0b5a1d](https://fairsharing.org/10.25504/FAIRsharing.0b5a1d)) links IDC to the record of the DICOM standard ([FAIRsharing.b7z8by](https://fairsharing.org/FAIRsharing.b7z8by)).
* **SciCrunch** lists IDC as RRID:SCR\_019127.
* **DataCite** has all 242 dataset DOIs in the state `findable` (query under [R1.1](#r1-1)).

## Accessible

### A1. Retrieval by identifier <a href="#a1" id="a1"></a>

Every series has a download URL (`series_with_download_url` under [F1](#f1)). Both of the following download one series without any credentials:

```bash
# by DICOM Series Instance UID, with the idc command installed by idc-index
idc download 1.3.6.1.4.1.9328.50.1.54657

# by its storage URL, with the s5cmd tool that idc-index also installs
s5cmd --no-sign-request cp 's3://idc-open-data/ec673959-b31f-47bb-acac-3a93fa8a2e28/*' .
```

### A2. Metadata of earlier versions <a href="#a2" id="a2"></a>

`prior_versions_index` lists the versions of series that earlier releases served and the current release does not. It is IDC metadata, built from the metadata of every earlier release.

```sql
SELECT
  COUNT(*)                         AS rows,
  COUNT(DISTINCT crdc_series_uuid) AS earlier_versions,
  COUNT(DISTINCT crdc_series_uuid)
    FILTER (WHERE SeriesInstanceUID IN (SELECT SeriesInstanceUID FROM index))     AS versions_of_current_series,
  COUNT(DISTINCT SeriesInstanceUID)
    FILTER (WHERE SeriesInstanceUID NOT IN (SELECT SeriesInstanceUID FROM index)) AS series_no_longer_in_idc,
  MIN(min_idc_version)             AS earliest_release,
  COUNT(*) FILTER (WHERE series_aws_url IS NULL) AS rows_without_aws_url
FROM prior_versions_index
```

| Column                       |    v25 |
| ---------------------------- | -----: |
| `rows`                       | 82,428 |
| `earlier_versions`           | 71,042 |
| `versions_of_current_series` | 57,782 |
| `series_no_longer_in_idc`    | 13,208 |
| `earliest_release`           |      2 |
| `rows_without_aws_url`       |      0 |

A version can occupy more than one row, which is why `rows` exceeds `earlier_versions`. Series revised since they were first released:

```sql
SELECT COUNT(*) FILTER (WHERE series_revised_idc_version > series_init_idc_version) AS series_revised
FROM index
```

The result is 49,304 series, 4.7% of the total. The metadata of every release, v1 to v25, is listed in the public bucket:

```bash
s5cmd --no-sign-request ls 's3://idc-open-metadata/bigquery_export/'
```

## Interoperable

### I1. DICOM throughout <a href="#i1" id="i1"></a>

```sql
SELECT COUNT(DISTINCT Modality)          AS modality_codes,
       COUNT(DISTINCT SOPClassUID)       AS sop_classes,
       COUNT(DISTINCT TransferSyntaxUID) AS transfer_syntax_values
FROM index
```

The result is 25 modality codes, 38 SOP classes (types of DICOM object) and 16 transfer syntax values. A series whose files use more than one transfer syntax is recorded with a combined value, such as `Explicit VR Little Endian,JPEG Baseline`; 71,783 series do so.

<details>

<summary>Series by modality, object type and transfer syntax</summary>

`SELECT Modality, COUNT(*) AS series FROM index GROUP BY Modality ORDER BY series DESC`

| Modality | Series  | Modality | Series |
| -------- | ------: | -------- | -----: |
| CT       | 270,830 | PR       |  1,193 |
| SR       | 270,687 | OT       |    709 |
| SEG      | 193,295 | RTDOSE   |    524 |
| MR       | 128,143 | RTPLAN   |    484 |
| SM       |  76,300 | NM       |    100 |
| MG       |  48,128 | REG      |     97 |
| RTSTRUCT |  19,366 | XA       |     41 |
| CR       |  12,534 | KO       |     40 |
| ANN      |   7,448 | RWV      |     20 |
| PT       |   5,929 | FUSION   |     18 |
| DX       |   3,712 | RF       |     12 |
| M3D      |   2,328 | XC       |      4 |
| US       |   2,249 |          |        |

`SELECT sop_class_name, COUNT(*) AS series FROM index GROUP BY sop_class_name ORDER BY series DESC`

| SOP class                                                  |  Series |
| ---------------------------------------------------------- | ------: |
| CT Image Storage                                           | 269,893 |
| Enhanced SR Storage                                        | 262,883 |
| Segmentation Storage                                       | 193,154 |
| MR Image Storage                                           | 126,452 |
| VL Whole Slide Microscopy Image Storage                    |  76,300 |
| Breast Tomosynthesis Image Storage                         |  26,294 |
| RT Structure Set Storage                                   |  19,366 |
| Computed Radiography Image Storage                         |  12,531 |
| Secondary Capture Image Storage                            |  10,904 |
| Digital Mammography X-Ray Image Storage - For Processing   |   7,979 |
| Microscopy Bulk Simple Annotations Storage                 |   7,448 |
| Positron Emission Tomography Image Storage                 |   5,885 |
| Comprehensive 3D SR Storage                                |   5,408 |
| Digital Mammography X-Ray Image Storage - For Presentation |   3,904 |
| Digital X-Ray Image Storage - For Presentation             |   3,703 |
| Encapsulated STL Storage                                   |   2,328 |
| Comprehensive SR Storage                                   |   2,118 |
| Ultrasound Multi-frame Image Storage                       |   1,843 |
| Raw Data Storage                                           |   1,398 |
| Grayscale Softcopy Presentation State Storage              |   1,086 |
| Parametric Map Storage                                     |     691 |
| RT Dose Storage                                            |     524 |
| RT Plan Storage                                            |     484 |
| Multi-frame Grayscale Byte Secondary Capture Image Storage |     320 |
| Acquisition Context SR Storage                             |     277 |
| Breast Projection X-Ray Image Storage - For Processing     |     209 |
| Breast Projection X-Ray Image Storage - For Presentation   |     204 |
| Label Map Segmentation Storage                             |     129 |
| Advanced Blending Presentation State Storage               |     107 |
| Spatial Registration Storage                               |      97 |
| Nuclear Medicine Image Storage                             |      95 |
| Ultrasound Image Storage                                   |      54 |
| Key Object Selection Document Storage                      |      40 |
| X-Ray Angiographic Image Storage                           |      39 |
| Real World Value Mapping Storage                           |      20 |
| X-Ray Radiofluoroscopic Image Storage                      |      12 |
| Digital X-Ray Image Storage - For Processing               |      11 |
| X-Ray Radiation Dose SR Storage                            |       1 |

`SELECT transfer_syntax_name, COUNT(*) AS series FROM index GROUP BY transfer_syntax_name ORDER BY series DESC`

| Transfer syntax                                     |  Series |
| --------------------------------------------------- | ------: |
| Explicit VR Little Endian                           | 865,370 |
| Implicit VR Little Endian                           |  78,162 |
| Explicit VR Little Endian,JPEG Baseline             |  60,397 |
| JPEG 2000 Lossless                                  |  22,126 |
| Explicit VR Little Endian,JPEG 2000                 |  10,569 |
| JPEG Baseline                                       |   4,719 |
| JPEG Lossless                                       |   1,373 |
| Explicit VR Little Endian,Implicit VR Little Endian |     431 |
| Explicit VR Little Endian,JPEG 2000 Lossless        |     383 |
| Explicit VR Big Endian                              |     258 |
| JPEG Extended                                       |     151 |
| Deflated Explicit VR Little Endian                  |     129 |
| JPEG-LS Lossless                                    |      97 |
| JPEG 2000                                           |      23 |
| Explicit VR Big Endian,Implicit VR Little Endian    |       2 |
| Implicit VR Little Endian,JPEG Extended             |       1 |

</details>

Exceptions, all in data as submitted:

* 260 series use the retired Explicit VR Big Endian transfer syntax, alone or with another.
* In the Lung-Fused-CT-Pathology collection, 18 series use the modality `FUSION`, which is not a DICOM defined term, and 12 CT images carry the modality `SEG`, which the CT Image object does not allow.

### I2. Vocabularies <a href="#i2" id="i2"></a>

**Segment codes.** Each segment of a segmentation codes what it represents in Segmented Property Type Code Sequence (0062,000F). `idc-index` keeps only the code meanings, so the coding schemes come from the BigQuery `segmentations` table, which lists the segments of every segmentation, read here from its parquet export. This runs in DuckDB directly, outside `idc-index`, and takes a few minutes:

```python
import duckdb

con = duckdb.connect()
con.sql("INSTALL httpfs; LOAD httpfs; SET s3_region = 'us-east-1';")
con.sql("""
SELECT SegmentedPropertyType.CodingSchemeDesignator         AS coding_scheme,
       COUNT(DISTINCT (SOPInstanceUID, SegmentNumber))      AS segments,
       COUNT(DISTINCT SeriesInstanceUID)                    AS series,
       COUNT(DISTINCT SegmentedPropertyType.CodeValue)      AS distinct_codes
FROM read_parquet('s3://idc-open-metadata/bigquery_export/idc_v25/segmentations/*.parquet')
GROUP BY ALL
ORDER BY segments DESC
""").df()
```

| Coding scheme       |  Segments | Series  | Distinct codes |
| ------------------- | --------: | ------: | -------------: |
| `SCT` (SNOMED CT)   | 9,682,773 | 176,031 |            116 |
| `SRT` (SNOMED-RT, retired) | 9,463 |  8,775 |             12 |
| `MRSC` (local)      |     8,225 |   8,225 |              2 |
| `NCIt`              |     4,366 |   4,137 |              3 |
| `DCM`               |       123 |     123 |              1 |
| **Total**           | **9,704,950** | **193,154** |        |

SNOMED CT codes 99.8% of segments. A series can use more than one scheme, so the series column does not add up to the total.

**Body Part Examined.** This compares the values in IDC with the defined terms in Tables L-1 to L-3 of PS3.16 Annex L (reading the page needs `pip install lxml`):

```python
import pandas as pd

values = client.sql_query("""
    SELECT BodyPartExamined, COUNT(*) AS series
    FROM index
    WHERE trim(BodyPartExamined) <> ''
    GROUP BY BodyPartExamined
""")

annex_l = pd.read_html(
    "https://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_L.html",
    match="Body Part Examined",
)
defined_terms = set()
for table in annex_l:
    if "Body Part Examined" in table.columns:
        defined_terms |= set(table["Body Part Examined"].dropna())

values["defined_term"] = values["BodyPartExamined"].isin(defined_terms)
values.groupby("defined_term")["series"].agg(values="count", series="sum")
```

Of 76 distinct values, 61 are defined terms and cover 574,115 of the 574,950 series that have a value (99.85%). The other 15 cover 835 series:

| Value              | Series | Value              | Series |
| ------------------ | -----: | ------------------ | -----: |
| `Left Breast`      |    217 | `UNKNOWN`          |     15 |
| `LEG`              |    208 | `SEG`              |     12 |
| `Right Breast`     |    179 | `TH CT CHEST WO C` |     11 |
| `BONYPELVIS`       |     92 | `CHEST (THORAX) W` |      6 |
| `Phantom`          |     51 | `OUTSIDE FIL`      |      3 |
| `OTHER`            |     18 | `WO INTER`         |      2 |
| `FUSION`           |     18 | `BD CT ABD WO_W C` |      2 |
|                    |        | `BODY`             |      1 |

**Segment Algorithm Type.** Segment Algorithm Type (0062,0008) is recorded for each segment, and DICOM allows only `AUTOMATIC`, `SEMIAUTOMATIC` and `MANUAL`. The `AlgorithmType` column of `seg_index` combines the distinct values in a series into one, such as `AUTOMATIC,MANUAL`:

```sql
SELECT AlgorithmType, COUNT(*) AS series,
       ROUND(100.0 * COUNT(*) / SUM(COUNT(*)) OVER (), 1) AS percent
FROM seg_index
GROUP BY AlgorithmType
ORDER BY series DESC
```

| Algorithm type            |  Series |    % |
| ------------------------- | ------: | ---: |
| `AUTOMATIC`               | 164,889 | 85.3 |
| `MANUAL`                  |  14,673 |  7.6 |
| `SEMIAUTOMATIC`           |  10,615 |  5.5 |
| `AUTOMATIC,MANUAL`        |   1,870 |  1.0 |
| `MANUAL,SEMIAUTOMATIC`    |     560 |  0.3 |
| `SEMIAUTOMATED`           |     462 |  0.2 |
| `AUTOMATIC,SEMIAUTOMATIC` |     214 |  0.1 |

`SEMIAUTOMATED` is not an allowed value; all 462 series are in the QIBA CT-1C collection.

### I3. References between series <a href="#i3" id="i3"></a>

```sql
WITH derived AS (
  SELECT 'SEG' AS object_type, segmented_SeriesInstanceUID AS referenced FROM seg_index
  UNION ALL
  SELECT 'RTSTRUCT', referenced_SeriesInstanceUID FROM rtstruct_index
  UNION ALL
  SELECT 'ANN', referenced_SeriesInstanceUID FROM ann_index
)
SELECT object_type,
       COUNT(*) AS series,
       COUNT(*) FILTER (WHERE trim(referenced) <> '') AS name_referenced_series,
       COUNT(*) FILTER (WHERE referenced IN (SELECT SeriesInstanceUID FROM index)) AS referenced_series_in_idc
FROM derived
GROUP BY object_type
ORDER BY series DESC
```

| Object type | Series  | Name the series they annotate | That series is in IDC |
| ----------- | ------: | ----------------------------: | --------------------: |
| SEG         | 193,283 |                       191,791 |               191,791 |
| RTSTRUCT    |  19,366 |                        19,366 |                19,366 |
| ANN         |   7,448 |                         7,448 |                 7,448 |

The two last columns mean different things for different object types:

* **RTSTRUCT and ANN** name the series they annotate directly, in RT Referenced Series Sequence (3006,0014) and Referenced Series Sequence (0008,1115) respectively. The last column checks that each series they name is in IDC.
* **SEG**: `seg_index` finds the source series by taking the first source image a segmentation references, in Referenced Series Sequence (0008,1115), Referenced Image Sequence (0008,1140) or Source Image Sequence (0008,2112), and looking it up in IDC (see the [SQL](https://github.com/ImagingDataCommons/idc-index-data/blob/main/assets/seg_index.sql)). The two columns are therefore equal by construction, and a segmentation that references no images cannot be told apart from one that references images outside IDC.

Segmentation series whose source series could not be identified, by where they came from:

```sql
SELECT i.collection_id,
       COALESCE(i.analysis_result_id, '(original data)') AS analysis_result,
       COUNT(*) AS series
FROM seg_index AS s
JOIN index AS i USING (SeriesInstanceUID)
WHERE coalesce(trim(s.segmented_SeriesInstanceUID), '') = ''
GROUP BY ALL
ORDER BY series DESC
```

| Collection                    | Analysis result         | Series | Contributed by | Converted to DICOM by       |
| ----------------------------- | ----------------------- | -----: | -------------- | --------------------------- |
| `qiba_ct_1c`                  | (original data)         |    462 | TCIA           | not applicable (DICOM data) |
| `remind`                      | (original data)         |    356 | TCIA           | not applicable (DICOM data) |
| `rider_lung_ct`               | `qiba_volct_1b`         |    320 | TCIA           | not documented              |
| `rider_pilot`                 | `qiba_volct_1b`         |    200 | TCIA           | not documented              |
| `spinal_multiple_myeloma_seg` | (original data)         |    142 | TCIA           | not applicable (DICOM data) |
| `nlst`                        | `nlstseg`               |      5 | IDC            | IDC                         |
| `prostatex`                   | `bamf_aimi_annotations` |      3 | IDC            | BAMF                        |
| `breast_mri_nact_pilot`       | (original data)         |      2 | TCIA           | not applicable (DICOM data) |
| `hcc_tace_seg`                | (original data)         |      1 | TCIA           | not applicable (DICOM data) |
| `rider_lung_ct`               | (original data)         |      1 | TCIA           | not applicable (DICOM data) |

The last two columns are from the provenance record of each source (see [R1.2](#r1-2)). Only the five `nlstseg` series were converted by IDC. Structured reports (SR) were not included in this check, and references were checked at series level.

## Reusable

### R1. Clinical data <a href="#r1" id="r1"></a>

```sql
SELECT COUNT(DISTINCT table_name)    AS clinical_tables,
       COUNT(DISTINCT collection_id) AS collections_with_clinical_data,
       COUNT(*)                      AS dictionary_entries
FROM clinical_index
```

The result is 235 tables for 100 of the 179 collections, described by 10,661 data dictionary entries.

### R1.1. Licenses <a href="#r1-1" id="r1-1"></a>

```sql
SELECT license_short_name,
       COUNT(*) AS series,
       ROUND(100.0 * COUNT(*) / SUM(COUNT(*)) OVER (), 2) AS percent_of_series,
       ROUND(SUM(series_size_MB) / 1e6, 2) AS size_TB
FROM index
GROUP BY license_short_name
ORDER BY series DESC
```

| License                                    |  Series | % of series | Size, TB |
| ------------------------------------------ | ------: | ----------: | -------: |
| CC BY 4.0                                  | 877,215 |       84.01 |    74.81 |
| CC BY 3.0                                  | 132,303 |       12.67 |    21.95 |
| CC BY-NC 4.0                               |  28,783 |        2.76 |     2.04 |
| CC BY-NC 3.0                               |   5,851 |        0.56 |     0.83 |
| NLM Terms and Conditions; May 21, 2019     |      39 |        0.00 |     0.31 |

As labelled, 96.68% of series allow commercial use. If the [licenses under review](README.md#r1-1) were all resolved in favour of the DOI record, the share would be 95.31%. Each of the 308 data sources listed in `collections_index` carries a license URL:

```sql
SELECT COUNT(*) AS sources, COUNT(src.license.license_url) AS with_license_url
FROM (SELECT unnest(sources) AS src FROM collections_index)
```

**Comparison with DataCite.** This reads the license of each dataset DOI from DataCite and compares its SPDX identifier with the license IDC records:

```python
import json
import urllib.request
from concurrent.futures import ThreadPoolExecutor

SPDX = {"CC BY 4.0": "cc-by-4.0", "CC BY 3.0": "cc-by-3.0",
        "CC BY-NC 4.0": "cc-by-nc-4.0", "CC BY-NC 3.0": "cc-by-nc-3.0"}

dois = client.sql_query("""
    SELECT lower(source_DOI) AS doi, license_short_name, COUNT(*) AS series,
           string_agg(DISTINCT collection_id, ', ') AS collections
    FROM index
    GROUP BY ALL
""")

def datacite(doi):
    with urllib.request.urlopen(f"https://api.datacite.org/dois/{doi}", timeout=60) as response:
        attributes = json.load(response)["data"]["attributes"]
    spdx = {r["rightsIdentifier"].lower() for r in attributes.get("rightsList") or []
            if (r.get("rightsIdentifierScheme") or "").upper() == "SPDX" and r.get("rightsIdentifier")}
    return attributes["state"], spdx

with ThreadPoolExecutor(max_workers=8) as pool:
    results = list(pool.map(datacite, dois["doi"]))

dois["datacite_state"] = [state for state, _ in results]
dois["datacite_spdx"] = [", ".join(sorted(spdx)) for _, spdx in results]
dois["agrees"] = [spdx == ({SPDX[lic]} if lic in SPDX else set())
                  for lic, (_, spdx) in zip(dois["license_short_name"], results)]
dois[~dois["agrees"]]
```

All 242 DOIs are `findable`. The licenses agree for 228 and disagree for 14, which are listed under [R1.1](README.md#r1-1) on the main page. The NLM terms have no SPDX identifier, so the NLM Visible Human DOI counts as agreeing when its DataCite record carries none.

### R1.2. Provenance <a href="#r1-2" id="r1-2"></a>

Each data source has a provenance record: who contributed it to IDC, who provided the source material, who de-identified it and who converted it to DICOM. It is IDC metadata, not part of the DICOM files. `idc-index` keeps it in `collections_index`, for each source of original data, and in `analysis_results_index`; in BigQuery it is in `original_collections_metadata` and `analysis_results_metadata`.

```sql
SELECT 'original data' AS kind, COUNT(*) AS sources, COUNT(src.provenance) AS with_provenance
FROM (SELECT unnest(sources) AS src FROM collections_index) WHERE src.source_type = 'original_data'
UNION ALL
SELECT 'analysis results', COUNT(*), COUNT(provenance) FROM analysis_results_index
```

All 216 sources of original data and all 26 analysis results have one. Joining them to the series shows that every series is covered:

```sql
WITH documented AS (
  SELECT lower(src.source_doi) AS doi
  FROM (SELECT unnest(sources) AS src FROM collections_index)
  WHERE src.provenance IS NOT NULL
  UNION
  SELECT lower(source_DOI) FROM analysis_results_index WHERE provenance IS NOT NULL
)
SELECT COUNT(*) AS series,
       COUNT(*) FILTER (WHERE lower(source_DOI) IN (SELECT doi FROM documented)) AS series_with_provenance_record
FROM index
```

The result is 1,044,191 of 1,044,191 series. How each segmentation was made is under [I2](#i2) (Segment Algorithm Type).

**Citations** are generated from the DOIs of a selection:

```python
citations = client.citations_from_selection(collection_id="tcga_brca")
len(citations)  # 6: five datasets and the IDC paper
```
