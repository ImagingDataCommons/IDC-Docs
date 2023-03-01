# Downloading data with s5cmd

[`s5cmd`](https://github.com/peak/s5cmd) is a very fast S3 and local filesystem execution tool, which, as experiments showed, is significantly faster than the Google-provided `gsutil`.

{% hint style="info" %}
Unlike `gsutil`, `s5cmd` does not currently verify file checksum on download (although, this feature appears to be added to the `v2.1.0` milestone). See related discussion in [https://github.com/peak/s5cmd/issues/398](https://github.com/peak/s5cmd/issues/398). If you need help verifying checksum of the downloaded files, please post a question on [IDC forum](https://discourse.canceridc.dev).
{% endhint %}

### Prerequisites

Install `s5cmd` following the instructions in [https://github.com/peak/s5cmd#installation](https://github.com/peak/s5cmd#installation).

You can verify if your setup was successful by running the following command: it should successfully download one file from IDC using `s5cmd.`

```shell
s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com cp s3://public-datasets-idc/eae91afc-1977-4728-9d6a-06f782c696d4.dcm .
```

If the command above completes successfully, you can proceed with download in 2 steps, as before, but with slight modifications.

### Step 1: Generate s5cmd compatible manifest

`s5cmd` manifest should contain the list of command copying individual files from your list that should have the following format (note the `s3` URL prefix!):

```
cp s3://<bucket>/<file1> .
cp s3://<bucket>/<file1> .
```

You can generate such manifest by just slightly modifying the query for preparing the manifest as described in [.](./ "mention") (here we demonstrate the query that selects a sample series, but you should adjust the selection based on your needs):

```sql
SELECT CONCAT("cp ",REPLACE(gcs_url, "gs://", "s3://"), " .") 
FROM bigquery-public-data.idc_current.dicom_all 
WHERE SeriesInstanceUID = "1.3.6.1.4.1.32722.99.99.298991776521342375010861296712563382046"
```

Save the query defining the manifest into a file named `s5cmd_query.txt`, and run the query to save the manifest into a file `s5cmd_manifest.txt`:

```shell
bq query --use_legacy_sql=false --format=csv --max_rows=20000000
< s5cmd_query.txt > s5cmd_manifest.txt
```

### Step 2: Download the files specified in the manifest

The following command will download the files specified in `s5cmd_manifest.txt` to the current directory:

```shell
s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com run s5cmd_manifest.txt
```
