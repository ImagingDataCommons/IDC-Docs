# Downloading data

{% hint style="success" %}
Egress of IDC data out of the cloud is free, sponsored by [Google Public Datasets Program](https://console.cloud.google.com/marketplace/product/gcp-public-data-idc/nci-idc-data)!&#x20;
{% endhint %}

In order to download the data from IDC, you first need to define the list of specific files you want to download. The list of files is defined by the Google Storage `gs://` URLs. Given that list of files, you can use the [Google Cloud SDK](https://cloud.google.com/sdk/docs/install) `gsutil` command to crossload files to a GCP VM, or to download files to your computer.&#x20;

{% hint style="info" %}
You will need to have [Google Cloud SDK](https://cloud.google.com/sdk/docs/install) installed on the machine you will be using to download IDC data.
{% endhint %}

### How to define the list of files for download&#x20;

**Using the IDC Portal**: see instructions [here](https://learn.canceridc.dev/portal/data-exploration-and-cohorts/understanding-cohorts) on how to define, save and export a cohort into BigQuery.&#x20;

**Using BigQuery**: you can utilize the [`dicom_all`](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=dicom\_all\&page=table) BigQuery table discussed in [this documentation article](https://learn.canceridc.dev/data/organization-of-data/files-and-metadata#bigquery-tables) to subset the files you need based on the DICOM metadata attributes as needed utilizing the SQL query interface. The `gcs_url` column contains Google Storage gs:// URLs that can be used to retrieve the files.

If you have a BigQuery table that has a column with `gcs_url` values (such as the BigQuery table with the manifest exported using IDC portal), you can fetch the content of that column into a file using the following GCP Cloud SDK command line (`tail -n +2` is used to skip the header of the exported column). You will need to substitute `MY_COHORT_TABLE` with the name of the BigQuery table with your manifest.

{% hint style="danger" %}
Make sure you adjust the `--max_rows` parameter in the below to be equal or exceed the number of rows in the result of the query, otherwise your list will be truncated!
{% endhint %}

```shell-session
bq query --format=csv  --max_rows=2000000 --use_legacy_sql=false \
  "SELECT gcs_url from \`MY_COHORT_TABLE\`" | tail -n +2 > gcs_urls.csv
```

You can also pass the query against [`dicom_all`](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=dicom\_all\&page=table) directly into the command above, if you do not have a cohort table. As an example, the query below will select `gcs_url` column for all instances that belong to collections that start with `cptac` and have `Modality` of `SM` (slide microscopy):

```shell-session
bq query --format=csv  --max_rows=2000000 --use_legacy_sql=false \
  "SELECT gcs_url FROM \`bigquery-public-data.idc_current.dicom_all\` where Modality = \"SM\"  and collection_id like \"cptac%\"" | tail -n +2 > gcs_urls.csv
```

### How to retrieve the files defined by Google Storage URLs

If you have the list of GCS URLs stored in a file on disk, you can use the following command:

```shell-session
$ cat gcs_urls.csv | gsutil -m cp -I .
```

The command above is quick and simple to get the small number of files from IDC, but it is not very efficient. You can implement parallel download of your cohort using `xargs` as follows:

Save the following into a shell script, such as `gsutil_download.sh` (here, `$DESTINATION` could be a local folder, or another GCP storage bucket):

```shell-session
$ echo "gsutil cp \$* $DESTINATION" > gsutil_download.sh
$ chmod +x gsutil_download.sh
```

Next, feed the file with the list of GCS URLs to `xargs`. You can experiment with the `-n` (number of GCS URLs passed to a single invocation of the download script) and `-P` (number of processes that will run the download script) parameters to optimize performance.

```shell-session
$ cat gcs_urls.csv | xargs -n 25 -P 10 ./gsutil_download.sh
```



