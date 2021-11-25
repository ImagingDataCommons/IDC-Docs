# Downloading data

{% hint style="info" %}
You will need to have a valid Google Cloud Platform (GCP) ProjectID with billing enabled to download files from IDC buckets (even though you will not be charged if you crossload data to a GCS VM). You can get a sponsored GCP project from IDC through the application form [here](https://learn.canceridc.dev/introduction/requesting-gcp-cloud-credits).&#x20;
{% endhint %}

In order to download the data from IDC, you first need to define the list of specific files you want to download. The list of files is defined by the Google Storage gs:// URLs. Given that list of files, you can use the Google Cloud SDK `gsutil` command to crossload files to a GCP VM, or to download files to your computer.&#x20;

{% hint style="danger" %}
Download of the IDC files from GCP to a non-GCP location will incur egress charges to the project you specify while downloading the data! Crossloading of the data to a GCP VM is free within North America regions.
{% endhint %}

### How to define the list of files for download&#x20;

**Using the IDC Portal**: see instructions [here](https://learn.canceridc.dev/portal/data-exploration-and-cohorts/understanding-cohorts) on how to define, save and export a cohort into BigQuery.&#x20;

**Using BigQuery**: you can utilize the [`dicom_all`](https://console.cloud.google.com/bigquery?p=canceridc-data\&d=idc\_current\&t=dicom\_all\&page=table) BigQuery table discussed in [this documentation article](https://learn.canceridc.dev/data/organization-of-data/files-and-metadata#bigquery-tables) to subset the files you need based on the DICOM metadata attributes as needed utilizing the SQL query interface. The `gcs_url` column contains Google Storage gs:// URLs that can be used to retrieve the files.

If you have a BigQuery table that has a column with `gcs_url` values (such as the BigQuery table with the manifest exported using IDC portal), you can fetch the content of that column into a file using the following GCP Cloud SDK command line (`tail -n +2` is used to skip the header of the exported column). You will need to substitute `MY_COHORT_ID` with the name of the BigQuery table with your manifest.

{% hint style="danger" %}
Make sure you adjust the `--max-rows` parameter in the below to be equal or exceed the number of rows in the result of the query, otherwise your list will be truncated!
{% endhint %}

```shell-session
bq query --format=csv  --max_rows=2000000 --use_legacy_sql=false \
  "SELECT gcs_url from \`MY_COHORT_ID\`" | tail -n +2 > gcs_urls.csv
```

### How to retrieve the files defined by Google Storage URLs

If you have the list of GCS URLs stored in a file on disk, you can use the following command (having saved the project ID you chose above in the `ProjectID` shell variable, or replacing the `$ProjectID` argument with the actual project ID, and assuming you want to skip the first line in the manifest if it's a header):

```shell-session
$ cat gcs_urls.txt | gsutil -u $ProjectID -m cp -I .
```

The command above is quick and simple to get the small number of files from IDC, but it is not very efficient. You can implement parallel download of your cohort using `xargs` as follows:

Save the following into a shell script, such as `gsutil_download.sh` (here, `$DESTINATION` could be a local folder, or another GCP storage bucket):

```shell-session
$ echo "gsutil -u $ProjectID cp $* $DESTINATION" > gsutil_download.sh
$ chmod +x gsutil_download.sh
```

Next, feed the file with the list of GCS URLs to `xargs`. You can experiment with the `-n` (number of GCS URLs passed to a single invocation of the download script) and `-P` (number of processes that will run the download script) parameters to optimize performance.

```shell-session
$ cat gcs_urls.txt | xargs -n 25 -P 10 ./gsutil_download.sh
```



