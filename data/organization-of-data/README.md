# Organization of data

IDC provides a variety of interfaces to access both the data (as files) and metadata (to subset files and build cohorts). The flow of data and the relationship between the various components IDC uses is summarized in the following figure.

{% embed url="https://docs.google.com/presentation/d/1UVpNVyVy3xIYLDnm4rtgAUmSu-uKQo5krekI9DSMT8o/edit?usp=sharing" %}

We maintain the following resources to enable access to IDC data:

* [Cloud storage buckets](./#files-and-metadata): files maintained by IDC are mirrored between Google and AWS public storage buckets that provide fee-free egress without requiring login. The buckets organize files by DICOM series, each series stored in a separate folder. Given the large overall size of data in IDC, you will likely need to use one of the search interfaces to identify relevant series first.
* BigQuery tables: collection-level metadata, DICOM metadata, [clinical data tables](clinical.md) available via SQL query interface.
* Python API: pip-installable [idc-index package](https://idc-index.readthedocs.io/en/latest/) provides a programmatic interface and command-line tools to search IDC data using most important metadata attributes, and to download files corresponding to the selected cohorts from the cloud buckets
* [REST API](/broken/pages/sJaxkFAmZQ06CPzQSpx9): alternative language-independent API for selecting subsets of data
* [DICOMweb](dicom-stores.md): DICOM files and metadata queries available from Google Healthcare DICOM stores
