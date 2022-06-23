# Downloading data

{% hint style="success" %}
Egress of IDC data out of the cloud is free, sponsored by [Google Public Datasets Program](https://console.cloud.google.com/marketplace/product/gcp-public-data-idc/nci-idc-data)!&#x20;
{% endhint %}

Download of data from IDC is a 2-step process covered on this page:

* **First step:** define the list of specific files you want to download. The list of files is defined by the Google Storage `gs://` URLs.&#x20;
* **Second step**: given that list of files, download files to your computer or to a cloud VM.&#x20;

{% hint style="info" %}
You will need to complete prerequisites described in [getting-started-with-gcp.md](../introduction/getting-started-with-gcp.md "mention") in order to be able to follow the instructions below!
{% endhint %}

### First step: define the list of files for download&#x20;

****[`dicom_all`](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=dicom\_all\&page=table) BigQuery table discussed in [this documentation article](https://learn.canceridc.dev/data/organization-of-data/files-and-metadata#bigquery-tables) can be used to subset the files you need based on the DICOM metadata attributes as needed utilizing the SQL query interface. The `gcs_url` column contains Google Storage gs:// URLs that can be used to retrieve the files.

Start with the query templates provided below, modify them based on your needs, and save the result in a file `my_query.txt`. The specific values for `PatientID`, `SeriesInstanceUID`, `StudyInstanceUID` are chosen to serve as examples.&#x20;

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

Next, use Google Cloud SDK `bq query` command (from command line) to run the query and save the result, which will be the list of GCP URLs that can be used to download the data.

```shell
bq query --use_legacy_sql=false --format=csv --max_rows=2000000 \
  < my_query.txt | tail -n +2 > gcs_urls.csv
```

{% hint style="danger" %}
Make sure you adjust the `--max_rows` parameter in the queries above to be equal or exceed the number of rows in the result of the query, otherwise your list will be truncated!
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

### Step two: download the files defined by GCS URLs

If you have the list of GCS URLs stored in a file, you can use the following command:

```shell-session
$ cat gcs_urls.csv | gsutil -m cp -I .
```

The command above is quick and simple to get the small number of files from IDC, but it is not very efficient if you need to download a lot of data. We recommend you use [s5cmd](https://github.com/peak/s5cmd) to download large number of files. Please refer to the [GCS download benchmarking Colab notebook](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/notebooks/download\_benchmarking.ipynb) for instructions on how to use s5cmd with GCS, and let us know on the [IDC user forum](https://discourse.canceridc.dev/) if you have any questions.

