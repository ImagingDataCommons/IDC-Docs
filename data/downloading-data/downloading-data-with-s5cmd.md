# Downloading data with `s5cmd`

Download of data from IDC is a 2-step process covered on this page:

* **Step 1:** create a manifest - a list of the storage bucket URLs of the files to be downloaded. if you want to download the content of the cohort defined in the IDC Portal, [export the `s5cmd` manifest fist](../../portal/cohort-manifests.md), and proceed to Step 2. Alternatively, you can use BigQuery SQL as discussed below to generate the manifest;
* **Step 2**: given the manifest, download files to your computer or to a cloud VM using `s5cmd` command line tool.

To learn more about using Google BigQuery SQL with IDC, check out part 3 of our ["Getting started" tutorial series](https://github.com/ImagingDataCommons/IDC-Tutorials/tree/master/notebooks/getting\_started), which demonstrates how to query and download IDC data!

### Step 1: Create the manifest

{% hint style="info" %}
You will need to complete prerequisites described in [getting-started-with-gcp.md](../../introduction/google-cloud-platform/getting-started-with-gcp.md "mention") in order to be able to execute the manifest generation queries below!
{% endhint %}

A download manifest can be created using either the IDC Portal, or by executing a BQ query. **If you have generated a manifest using the IDC Portal, as discussed** [**here**](../../portal/cohort-manifests.md)**, proceed to Step 2!** In the remainder of this section we describe creating a manifest from a BigQuery query.

The [`dicom_all`](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=dicom\_all\&page=table) BigQuery table discussed in [this documentation article](https://learn.canceridc.dev/data/organization-of-data/files-and-metadata#bigquery-tables) can be used to subset the files you need based on the DICOM metadata attributes as needed, utilizing the SQL query interface. The `gcs_url` and `aws_url` columns contain Google Cloud Storage and AWS S3 URLs, respectively, that can be used to retrieve the files.

Start with the query templates provided below, modify them based on your needs, and save the result in a file `query.txt`. The specific values for `PatientID`, `SeriesInstanceUID`, `StudyInstanceUID` are chosen to serve as examples.&#x20;

You can use IDC Portal to identify items of interest, or you can use SQL queries to subset your data using any of the DICOM attributes. You are encouraged to use the [BigQuery console](https://console.cloud.google.com/bigquery) to test your queries and explore the data first!

Queries below demonstrate how to get the Google Storage URLs to download cohort files.

{% code overflow="wrap" %}
```sql
# Select all files from GCS for a given PatientID
SELECT DISTINCT(CONCAT("cp s3://", SPLIT(gcs_url,"/")[SAFE_OFFSET(2)], "/", crdc_series_uuid, "/* .")) 
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE PatientID = "LUNG1-001"
```
{% endcode %}

{% code overflow="wrap" %}
```sql
# Select all files from GCS for a given collection
SELECT DISTINCT(CONCAT("cp s3://", SPLIT(gcs_url,"/")[SAFE_OFFSET(2)], "/", crdc_series_uuid, "/* .")) 
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE collection_id = "nsclc_radiomics"
```
{% endcode %}

{% code overflow="wrap" %}
```sql
# Select all files from GCS for a given DICOM series
SELECT DISTINCT(CONCAT("cp s3://", SPLIT(gcs_url,"/")[SAFE_OFFSET(2)], "/", crdc_series_uuid, "/* .")) 
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE SeriesInstanceUID = "1.3.6.1.4.1.32722.99.99.298991776521342375010861296712563382046"
```
{% endcode %}

{% code overflow="wrap" %}
```sql
# Select all files from GCS for a given DICOM study
SELECT DISTINCT(CONCAT("cp s3://", SPLIT(gcs_url,"/")[SAFE_OFFSET(2)], "/", crdc_series_uuid, "/* .")) 
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE StudyInstanceUID = "1.3.6.1.4.1.32722.99.99.239341353911714368772597187099978969331"
```
{% endcode %}

If you want to download the files corresponding to the cohort from AWS instead of GCP, substitute aws`_url` for gc`s_url` in the `SELECT` statement of the query, such as in the following SELECT clause:

{% code overflow="wrap" %}
```sql
SELECT DISTINCT(CONCAT("cp s3://", SPLIT(aws_url,"/")[SAFE_OFFSET(2)], "/", crdc_series_uuid, "/* .")) 
```
{% endcode %}

Next, use a Google Cloud SDK `bq query` command (from command line) to run the query and save the result into a manifest file, which will be the list of GCP URLs that can be used to download the data.

{% code overflow="wrap" %}
```shell
bq query --use_legacy_sql=false --format=csv --max_rows=20000000 < query.txt > manifest.txt
```
{% endcode %}

{% hint style="danger" %}
Make sure you adjust the `--max_rows` parameter in the queries above to be equal or exceed the number of rows in the result of the query, otherwise your list will be truncated!&#x20;
{% endhint %}

For any of the queries, you can get the count of rows to confirm that the `--max_rows` parameter is sufficiently large (use the [BigQuery console](https://console.cloud.google.com/bigquery) to run these queries):

```sql
# count the number of rows
SELECT COUNT(DISTINCT(crdc_series_uuid)) 
FROM bigquery-public-data.idc_current.dicom_all 
WHERE collection_id = "nsclc_radiomics"
```

You can also get the total disk space that will be needed for the files that you will be downloading:

```sql
# calculate the disk size in GB needed for the files to be downloaded
SELECT ROUND(SUM(instance_size)/POW(1024,3),2) as size_GB 
FROM bigquery-public-data.idc_current.dicom_all 
WHERE collection_id = "nsclc_radiomics"
```

### Step 2: Download the files defined by the manifest

[`s5cmd`](https://github.com/peak/s5cmd) is a very fast S3 and local filesystem execution tool that can be used for accessing IDC buckets and downloading files both from GCS and AWS.

Install `s5cmd` following the instructions in [https://github.com/peak/s5cmd#installation](https://github.com/peak/s5cmd#installation).

You can verify if your setup was successful by running the following command: it should successfully download one file from IDC.

{% code overflow="wrap" %}
```shell
s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com cp s3://public-datasets-idc/cdac3f73-4fc9-4e0d-913b-b64aa3100977/902b4588-6f10-4342-9c80-f1054e67ee83.dcm .
```
{% endcode %}

Once `s5cmd` is installed, you can use `s5cmd run` command to download the files corresponding to the manifest.&#x20;

If you defined manifest that references GCP buckets:

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">s5cmd --no-sign-request <a data-footnote-ref href="#user-content-fn-1">--endpoint-url https://storage.googleapis.com</a> run manifest_file_name
</code></pre>

If you defined manifest that references AWS buckets:

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">s5cmd --no-sign-request <a data-footnote-ref href="#user-content-fn-2">--endpoint-url https://s3.amazonaws.com</a> run manifest_file_name
</code></pre>

{% hint style="info" %}
If you created the manifest using IDC Portal, you will have the instructions to install `s5cmd` and the exact command to download its content in the header of the manifest, which will look like this:

{% code overflow="wrap" %}
```
# To download the files in this manifest, first install s5cmd (https://github.com/peak/s5cmd),
# then run the following command:
# s5cmd --no-sign-request --endpoint-url https://s3.amazonaws.com run cohorts_996_20230505_72608_aws.s5cmd
```
{% endcode %}
{% endhint %}

[^1]: Use this endpoint for accessing GCS buckets

[^2]: Use this endpoint for accessing AWS buckets
