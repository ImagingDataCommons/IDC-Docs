# Using a BQ Manifest to Load DICOM Files onto a VM

Once a manifest has been created, typically the next step is to load the files onto a VM for analysis, and the easiest way to do this is to create your manifest in a BigQuery table and then use that to direct the file loading onto a VM. This guide shows how this can be done,

## Step 1: Export a file manifest for your cohort into BigQuery.

The first step is to [export a file manifest for a cohort into BigQuery](../../portal/data-exploration-and-cohorts/#exporting-to-bigquery). You will want to copy this table into the project where you are going to run your VM. Do this using the Google BQ console, since the exported table can be accessed only using your personal credentials provided by your browser. The table copy living in the VM project will be readable by the service account running your VM.

## Step 2: Start up a VM

Start up your VM. If you have many files, you will want to speed the loading process by using a VM with multiple CPUs. Google describes the various [machine types](https://cloud.google.com/compute/docs/machine-types), but is not very specific about ingress bandwidth. However, in terms of published egress bandwidth, the larger machines certainly have more. Experimentation showed that an n2-standard-8 \(8 vCPUs, 32 GB memory\) machine could load 20,000 DICOM files in 2 minutes and 32 secconds, using 16 threads on 8 CPUs. That configuration reached a peak throughput of 68 MiB/s.

You also need to insure the machine has enough disk space. One of the checks in the script provided below is to calculate the total file load size. You might want to run that portion of the script and resize the disk as needed before actually doing the load.

## Step 3: Install the code provided

[This Python script](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/scripts/pullManifestToVM.py) performs the following steps:

* Performs a query on the specified BigQuery manifest table and creates a local manifest file on your VM.
* Performs a query that maps the GCS URLs of each file into DICOM hierarchical directory paths, and writes this out as a local TSV file on your VM.
* Performs a query that calculates the total size of all the downloads, and reports back if there is sufficient space on the filesystem to continue.
* Uses a multi-threaded bucket reader to pull the files from the GCS buckets and places them in the appropriate DICOM hierarchical directory.

To install the code on your VM and then setup the environment:

```text
sudo apt-get install -y git # If you have a fresh VM and need git:
cd ~
git clone https://github.com/ImagingDataCommons/IDC-Examples.git
cd IDC-Examples/scripts
chmod u+x *.sh
./setupVM.sh
```

You then need to customize the settings in the script:

```text
    TABLE = 'your-project-id.your-dataset.your-manifest-table' # BQ table with your manifest
    MANIFEST_FILE = '/path-to-your-home-dir/BQ-MANIFEST.txt' # Where will the manifest file go
    PATHS_TSV_FILE = '/path-to-your-home-dir/PATHS.tsv' # Where will the path file go
    TARG_DIR = '/path-to-your-home-dir/destination' # Has to be on a filesystem with enough space. Directory should exist.
    PAYING = 'your-project-id' # Needed for IDC requester pays buckets though it is free to crossload to a cloud VM
    THREADS = 16 # (2 * number) of cpus seems to work best
```

Finally, run the script:

```text
~/IDC-Examples/scripts/runManifestPull.sh
```

