# Files and metadata

## Storage Buckets

{% hint style="info" %}
Storage Buckets are basic containers in Google Cloud Storage and AWS S3 that provide storage for data objects (you can read more about the relevant terms in the Google Cloud Storage documentation [here](https://cloud.google.com/storage/docs/key-terms) and in S3 [here](https://aws.amazon.com/s3/)).
{% endhint %}

All IDC DICOM file data for all IDC data versions across all of the [collections hosted by IDC](https://imaging.datacommons.cancer.gov/collections/) are maintained in Google Cloud Storage (GCS) and AWS S3 (S3). Currently all DICOM files are maintained in buckets that allow for free egress within or out of the cloud. This is enabled through the partnership of IDC with [Google Public Data Program](https://console.cloud.google.com/marketplace/product/gcp-public-data-idc/nci-idc-data) and the [AWS Open Data Sponsorship Program](https://registry.opendata.aws/nci-imaging-data-commons/).

{% hint style="warning" %}
Note that only (versions of) DICOM instances have associated files (as discussed in [DICOM Data Model](../../dicom/data-model.md). There are no per-series or per-study files.
{% endhint %}

The object namespace is hierarchical, where, for each version of a DICOM instance having instance UUID \<instance\_uuid> in a version of a series version having UUID \<series\_uuid>, the file name is:

`<series_uuid>/<instance_uuid>.dcm`

Corresponding files have the same object name in GCS and S3, though the name of the containing buckets will be different.

### UIDs and UUIDs explained with an example

Consider an instance in the CPTAC-CM collection that has this `SOPInstanceUID`:  `1.3.6.1.4.1.5962.99.1.171941254.777277241.1640849481094.35.0`\


It is in a series having this `SeriesInstanceUID`:\
`1.3.6.1.4.1.5962.99.1.171941254.777277241.1640849481094.2.0`

The instance and series were added to the IDC Data set in IDC version 7. At that point, the instance was assigned UUID:\
`5dce0cf0-4694-4dff-8f9e-2785bf179267`\
and the series was assigned this UUID:\
`e127d258-37c2-47bb-a7d1-1faa7f47f47a`

In IDC version 10, a revision of this instance was added (keeping its original `SOPInstanceUID`), and assigned this UUID:\
`21e5e9ce-01f5-4b9b-9899-a2cbb979b542`

Because this instance was revised, the series containing it was implicitly revised. The revised series was thus issued a new UUID:\
`ee34c840-b0ca-4400-a6c8-c605cef17630`

Thus, the initial version of this instance has this file name:\
`e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm`\
and the revised version of the instance has the this file name:\
`ee34c840-b0ca-4400-a6c8-c605cef17630/21e5e9ce-01f5-4b9b-9899-a2cbb979b542.dcm`

Both versions of the instance are in both AWS and GCS buckets.&#x20;

{% code title="AWS bucket example" overflow="wrap" %}
```bash
s5cmd --no-sign-request ls s3://idc-open-data/e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
2023-04-09 11:49:55    3308170 5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
```
{% endcode %}

{% code title="GCS bucket example" overflow="wrap" %}
```bash
s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com ls s3://public-datasets-idc/e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
   3308170  2023-04-01T01:21:31Z  gs://public-datasets-idc/e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
TOTAL: 1 objects, 3308402 bytes (3.16 MiB)
```
{% endcode %}

Note that GCS and AWS bucket names are different. In fact, DICOM instance data is distributed across multiple buckets in both GCS and AWS. We will discuss obtaining GCS and AWS URLs more a little later.&#x20;

{% hint style="info" %}
It is possible that a series is revised, but one or more instances in the series are not revised. For example if a single instance in a series (assume the series has a uuid \<series\_uuid\_old>) is revised, that instance gets a new UUID, and there is implicitly a new version of the series, which gets a new UUID (call it \<series\_uuid\_new>). If an instance that is not revised has UUID \<invariant\_instance\_uuid>, then its corresponding file in cloud storage will the have name:\
`<series_uuid_old>/<invariant_instance_uuid>.dcm` in the "old" series. But, because that same instance version is in the revised series, there must also be a file in cloud storage named:\
`<series_uuid_new>/<invariant_instance_uuid>.dcm`\
The result will be two distinct but identical files.
{% endhint %}

Utilities like gsutil, s3 and s5cmd "understand" the implied hierarchy in these file names. Thus the series UUID now acts like the name of a directory that contains all the instance versions in the series version:

{% code overflow="wrap" %}
```bash
s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com ls s3://public-datasets-idc/ee34c840-b0ca-4400-a6c8-c605cef17630/
2023/04/01 03:00:34           1719696 18c206a6-2db4-45cd-89a2-e83273a38f42.dcm
2023/04/01 03:00:36           3308402 21e5e9ce-01f5-4b9b-9899-a2cbb979b542.dcm
2023/04/01 01:50:29          29477804 3cfc3da3-8389-49f6-a6ee-6ba6406f639e.dcm
2023/04/01 01:50:27         214715792 428590a0-816c-4041-a3ae-676a68411794.dcm
2023/04/01 03:00:30           2301902 57ff4432-c29d-4ccf-964c-0b421302add3.dcm
2023/04/01 03:00:33           3540080 77ff406a-a236-4846-83dd-ae3bd7a6bc71.dcm
```
{% endcode %}

and similarly for AWS buckets, thus making it easy to transfer all instances in a series from the cloud.

Because file names are more or less opaque, the user will not typically select files by listing the contents of a bucket. Instead, one should use either the IDC Portal or IDC BigQuery tables to identify items of interest and, then, generate a manifest of objects that can be passed to a utility like s5cmd.

## BigQuery Tables and Views

{% hint style="info" %}
Google [BigQuery (BQ)](https://cloud.google.com/bigquery) is a massively-parallel analytics engine ideal for working with tabular data. Data stored in BQ can be accessed using [standard SQL](https://cloud.google.com/bigquery/docs/reference/standard-sql/enabling-standard-sql) queries.
{% endhint %}

The BigQuery tables and views enable the researcher to reconstruct the DICOM hierarchy as it exists for any given IDC version and to perform searches against the metadata of the instances in that IDC version. Various other tables and views contain clinical and other metadata that may be helpful to the researcher as enumerated below.

The set of tables and views corresponding to an IDC version are collected in a single BQ dataset per IDC version, `bigquery-public-data.idc_<idc_version_number>` where `bigquery-public-data` is the project in which the dataset is hosted. As an example, the BQ tables for IDC version 4 are in the `bigquery-public-data.idc_v4` dataset.

In addition to the per-version datasets, the `bigquery-public-data.idc-current` dataset consists of a set of BQ views. There is a view in idc-current for each table or view in the BQ data set corresponding to the current IDC release. Each such view in `bigquery-public-data.idc-current` is named identically to some table or view in the `bigquery-public-data.idc_<idc_version_number>` dataset and is essentially an alias of that table or view.&#x20;

Several Google BigQuery (BQ) tables support searches against metadata extracted from the DICOM data files. Additional BQ tables define the composition of each IDC data version. We maintain several additional tables that curate non-DICOM metadata (e.g., attribution of a given item to a specific collection and DOI, collection-level metadata, etc).

The set of BQ tables and views has grown over time. The enumeration below documents the BQ tables and views as of IDC V14. Some of these tables will not be found in earlier IDC BQ datasets.

#### auxiliary\_metadata

This table defines the contents of the corresponding IDC version. There is a row for each instance in the version. We group the attributes for convenience:\
\
Collection attributes:

* `tcia_api_collection_id:` The ID, as accepted by the TCIA API, of the original data collection containing this instance (will be Null for collections not sourced from TCIA)
* `idc_webapp_collection_id:` The ID, as accepted by the IDC web app, of the original data collection containing this instance
* `collection_id:` The ID, as accepted by the IDC web app. Duplicate of `idc_webapp_collection_id`
* `collection_timestamp:` Datetime when the IDC data in the collection was last revised
* `collection_hash`: md5 hash of the of this version of the collection containing this instance
* `collection_init_idc_version:` The IDC version in which the collection containing this instance first appeared
* `collection_revised_idc_version:` The IDC version in which this version of the collection containing this instance first appeared

Patient attributes:

* `submitter_case_id:`The Patient ID assigned by the submitter of this data. This is the same as the DICOM PatientID
*   `idc_case_id:`IDC generated UUID that uniquely identifies the patient containing this instance

    This is needed because DICOM PatientIDs are not required to be globally unique
* `patient_hash`: md5 hash of this version of the patient/case containing this instance
* `patient_init_idc_version:` The IDC version in which the patient containing this instance first appeared
* `patient_revised_idc_version:` The IDC version in which this version of the patient/case containing this instance first appeared

Study attributes:

* `StudyInstanceUID:` DICOM UID of the study containing this instance
* `study_uuid:`IDC assigned UUID that identifies a version the the study containing this instance.
* `study_instances:` The number instances in the study containing this instance
* `study_hash`: md5 hash of the data in this version of the study containing this instance
* `study_init_idc_version:` The IDC version in which the study containing this instance first appeared
* `study_revised_idc_version:` The IDC version in which this version of the study containing this instance first appeared

Series attributes:

* `SeriesInstanceUID:` DICOM UID of the series containing this instance
* `series_uuid:`IDC assigned UUID that identifies the version of the series containing this instance
* `source_doi:`The DOI of an information page corresponding to the original data collection or analysis results that is the source of this instance
* `source_url:`The URL of an information page that describes the original collection or analysis result that is the source of this instance
* `series_instances:` The number of instances in the series containing this instance
* `series_hash`: md5 hash of the data in the this version of the series containing this instance
* `access:` Collection access status: 'Public' or 'Limited'. (Currently all data is 'Public')
* `series_init_idc_version:` The IDC version in which the series containing this instance first appeared
* `series_revised_idc_version:` The IDC version in which this version of the series containing this instance first appeared

Instance attributes:

* `SOPInstanceUID:` DICOM UID of this instance.
* `instance_uuid:`IDC assigned UUID that identifies the version of this instance.
* `gcs_url:` The GCS URL of a file containing the version of this instance that is identified by this `series_uuid/instance_uuid`
* `aws_url:` The AWS URL of a file containing the version of this instance that is identified by this `series_uuid/instance_uuid`
* `instance_hash`: the md5 hash of this version of this instance
* `instance_size:` the size, in bytes, of this version of this instance
* `instance_init_idc_version:` The IDC version in which this instance first appeared
* `instance_revised_idc_version:` The IDC version in which this version of this instance first appeared
* `license_url:` The URL of a web page that describes the license governing this version of this instance
* `license_long_name:` A long form name of the license governing this version of this instance
* `license_short_name:` A short form name of the license governing this version of this instance

#### `dicom_metadata`

Each row in the `dicom_metadata` table holds the DICOM metadata of an instance in the corresponding IDC version. There is a single row for each DICOM instance in the corresponding IDC version.\
IDC utilizes the standard capabilities of the Google Healthcare API to extract all of the DICOM metadata from the hosted collections into a single BQ table. Conventions of how DICOM attributes of various types are converted into BQ form are covered in the [Understanding the BigQuery DICOM schema](https://cloud.google.com/healthcare/docs/how-tos/dicom-bigquery-schema) Google Healthcare API documentation article. IDC users can `dicom_metadata` to conduct detailed explorations of the metadata content, and build cohorts using fine-grained controls not accessible from the IDC portal. (However, the `dicom_all` table, described below, is probably a better choice for such explorations.) The schema is too large to document here. Refer to the BQ table and the above referenced documentation.

{% hint style="warning" %}
Due to the existing limitations of Google Healthcare API, not all of the DICOM attributes are extracted and are available in BigQuery tables. Specifically:

* sequences that have more than 15 levels of nesting are not extracted (see [https://cloud.google.com/bigquery/docs/nested-repeated](https://cloud.google.com/bigquery/docs/nested-repeated)) - we believe this limitation does not affect the data stored in IDC
* sequences that contain around 1MiB of data are dropped from BigQuery export and RetrieveMetadata output currently. 1MiB is not an exact limit, but it can be used as a rough estimate of whether or not the API will drop the tag (this limitation was not documented as of writing this) - we know that some of the instances in IDC will be affected by this limitation. The fix for this limitation is targeted for sometime in 2021, according to the communication with Google Healthcare support.
{% endhint %}

#### original\_collections\_metadata

This table is comprised of IDC data collection-level metadata for the original TCIA data collections hosted by IDC, for the most part corresponding to the content available in [this table at TCIA](https://www.cancerimagingarchive.net/collections/). One row per collection:

* `tcia_api_collection_id:` The collection ID as is accepted by the TCIA AP
* `tcia_wiki_collection_id:` The collection ID as on the TCIA wiki page
* `idc_webapp_collection_id:`The collection ID as accepted by the IDC web app
* `Program:` The program to which this collection belongs
* `Updated:` Most recent update date reported by the collection source
* `Status:`Collection status: "Ongoing" or "Complete"
* `Access:`Collection access conditions: "Limited" or "Public"
* `ImageType:` Enumeration of image types/modalities in the collection
* `Subjects:`Number of subjects in the collection
* `DOI:`DOI that can be resolved at doi.org to the TCIA wiki page for this collection
* `URL:`URL of an information page for this collection
* `CancerType:`Collection source(s) assigned cancer type of this collection
* `SupportingData:`Type(s) of additional data available
* `Species:` Species of collection subjects
* `Location:`Body location that was studied
* `Description:` Description of the collection (HTML format)
* `license_url:` The URL of a web page that describes the license governing this collection
* `license_long_name:` A long form name of the license governing this collection
* `license_short_name:` A short form name of the license governing this collection

#### analysis\_results\_metadata

Metadata for the TCIA analysis results hosted by IDC, for the most part corresponding to the content available in [this table at TCIA](https://www.cancerimagingarchive.net/tcia-analysis-results/). One row per analysis result:

* `ID:` Results ID
* `Title:` Descriptive title
* `DOI:`DOI that can be resolved at doi.org to the TCIA wiki page for this analysis result
* `CancerType:`TCIA assigned cancer type of this analysis result
* `Location:`Body location that was studied
* `Subjects:`Number of subjects in the analysis result
* `Collections:` Original collections studied
* `AnalysisArtifactsonTCIA:` Type(s) of analysis artifacts generated
* `Updated:` Data when results were last updated
* `license_url:` The URL of a web page that describes the license governing this collection
* `license_long_name:` A long form name of the license governing this collection
* `license_short_name:` A short form name of the license governing this collection
* `description:` Description of analysis result

#### version\_metadata

&#x20;Metadata for each IDC version, one row per version:

* idc\_version: IDC version number
* version\_hash: MD5 hash of hashes of collections in this version
* version\_timestamp: Version creation timestamp

The following tables and views consist of metadata derived from one or more other IDC tables tables for convenience of the user. For each such table, `<table_name>`, there is also a corresponding view, `<table_name>_view`, that, when queried, generates an equivalent table. These views are intended as a reference; each view's SQL is available to be used for further investigation.

Several of these tables/views are discussed more completely [here](../../dicom/derived-objects/).

#### dicom\_all, dicom\_all\_view

All date from `dicom_metadata` together with selected date from the `auxiliary_metadata`, `original_collections_metadata`, and `analysis_results_metadata` tables

#### segmentations, segmentations\_view

Attributes of the segments stored in DICOM Segmentation objects

#### measurement\_groups, measurement\_groups\_view

Measurement group sequences extracted from DICOM SR TID1500 objects

#### qualitative\_measurements, qualitative\_measurements\_view

Coded evaluation results extracted from the DICOM SR TID1500 objects

#### quantitative\_measurements, quantitative\_measurements\_view

Quantitative evaluation results extracted from the DICOM SR TID1500 objects

#### dicom\_metadata\_curated,  dicom\_metadata\_curated\_view

Curated columns from dicom\_metadata

#### dicom\_metadata\_curated\_series\_level, dicom\_metadata\_curated\_series\_level\_view

Curated columns from dicom\_metadata that have been aggregated/cleaned up to describe content at the series level

#### idc\_pivot\_v\<idc\_version>

A view that is the basis for the queries performed by the IDC web app.&#x20;



### Collection-specific BigQuery tables

Most clinical data is found in the [idc\_v\<idc\_version>\_clinical datasets](clinical.md). However, a few tables of clinical data are found in the idc\_v\<idc\_version> datasets.

#### TCGA

The following tables contain TCGA-specific metadata:

* `tcga_biospecimen_rel9:` biospecimen metadata
* `tcga_clinical_rel9:` clinical metadata

#### NLST

{% hint style="info" %}
IDC hosts a subset of the NLST clinical data, which was cleared for public sharing. If you need the full clinical data, please visit the [Cancer Data Access System (CDAS) system](https://biometry.nci.nih.gov/cdas/learn/nlst/trial-summary/).
{% endhint %}

The following tables contain NLST specific metadata. The detailed schema of those tables is available from the [TCIA NLST collection page](https://doi.org/10.7937/tcia.hmq8-j677).

* `nlst_canc`: "Lung Cancer"
* `nlst_ctab`: "SCT Abnormalities"
* `nlst_ctabc`: "SCT Comparison Abnormalities"
* `nlst_prsn`: "Participant"
* `nlst_screen`: "SCT Screening"

## DICOM Stores

IDC utilizes a single Google Healthcare DICOM store to host all of the instances in the current IDC version. That store, however, is primarily intended to support visualization of the data using the OHIF and Slim viewers. At this time, we do not support access of the hosted data via DICOMWeb interface by IDC users. See more details in the [discussion here](https://discourse.canceridc.dev/t/dicomweb-access-to-hosted-collections/69), and please comment about your use case if you have a need to access data via the DICOMweb interface.
