# Downloading data

{% hint style="warning" %}
You will need to complete prerequisites described in [getting-started-with-gcp.md](../../introduction/getting-started-with-gcp.md "mention") in order to be able to follow the instructions below!
{% endhint %}

IDC does not have an interactive point-and-click download application! If you want to download data from IDC you will need to use command line interface (Terminal on Mac/Linux or Command prompt on Windows).

Download of data from IDC is a 2-step process covered on this page:

* **Step 1:** create the manifest - the list of files defined by the Google Storage `gs://` URLs;
* **Step 2**: given that list of files, download files to your computer or to a cloud VM.&#x20;

If you are analyzing IDC data in Google Colab, check out our [Colab cookbook notebook](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/notebooks/cookbook.ipynb) that includes examples of how to query and download IDC data!

### Step 1: Create the manifest&#x20;

****[`dicom_all`](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=dicom\_all\&page=table) BigQuery table discussed in [this documentation article](https://learn.canceridc.dev/data/organization-of-data/files-and-metadata#bigquery-tables) can be used to subset the files you need based on the DICOM metadata attributes as needed utilizing the SQL query interface. The `gcs_url` column contains Google Storage gs:// URLs that can be used to retrieve the files.

Start with the query templates provided below, modify them based on your needs, and save the result in a file `query.txt`. The specific values for `PatientID`, `SeriesInstanceUID`, `StudyInstanceUID` are chosen to serve as examples.&#x20;

You can use IDC Portal to identify items of interest, or you can use more complex SQL queries to subset your data using any of the DICOM attributes. You are encouraged to use [BigQuery console](https://console.cloud.google.com/bigquery) to test your queries and explore the data first!

```sql
# Select all files for a given PatientID
SELECT gcs_url
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE PatientID = "LUNG1-001"
```

```sql
# Select all files for a given collection
SELECT gcs_url
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE collection_id = "nsclc_radiomics"
```

```sql
# Select all files for a given DICOM series
SELECT gcs_url
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE SeriesInstanceUID = "1.3.6.1.4.1.32722.99.99.298991776521342375010861296712563382046"
```

```sql
# Select all files for a given DICOM study
SELECT gcs_url
FROM `bigquery-public-data.idc_current.dicom_all`
WHERE StudyInstanceUID = "1.3.6.1.4.1.32722.99.99.239341353911714368772597187099978969331"
```

If you defined the content you want to download as a cohort in IDC [Broken link](broken-reference "mention"), substitute the table with the cohort manifest exported into BigQuery in the following query:

```sql
# select gcs_url from the cohort exported in to BigQuery
# Replace the table name in FROM with the table containing your exported manifest!
SELECT gcs_url
FROM `canceridc-user-data.user_manifests.manifest_cohort_643_20220628_165636`
```

Next, use Google Cloud SDK `bq query` command (from command line) to run the query and save the result into a manifest file, which will be the list of GCP URLs that can be used to download the data.

```shell
bq query --use_legacy_sql=false --format=csv --max_rows=20000000 \
  < query.txt > manifest.txt
```

{% hint style="danger" %}
Make sure you adjust the `--max_rows` parameter in the queries above to be equal or exceed the number of rows in the result of the query, otherwise your list will be truncated!&#x20;
{% endhint %}

For any of the queries, you can get the count of rows to confirm that the `--max_rows` parameter is sufficiently large (use the [BigQuery console](https://console.cloud.google.com/bigquery) to run these queries):

```sql
# count the number of rows
SELECT COUNT(gcs_url) 
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

If you have the list of GCS URLs stored in a file, you can use the following command to download them into the current folder:

```shell-session
cat manifest.txt | gsutil -m cp -I .
```

{% hint style="warning" %}
Windows users will need to use `type` command in place of `cat`, since the latter is not available in the command line prompt on Windows:

`type manifest.txt | gsutil -m cp -I .`
{% endhint %}

The command above is quick and simple to get the small number of files from IDC, but it is not very efficient if you need to download a lot of data. We recommend you use [s5cmd](https://github.com/peak/s5cmd) to download large number of files. Read on to the next section to learn how to use \`s5cmd\` with IDC data: [downloading-data-with-s5cmd.md](downloading-data-with-s5cmd.md "mention").
