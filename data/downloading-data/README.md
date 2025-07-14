# Downloading data

{% hint style="info" %}
If you have questions or feedback about the download tools provided by IDC, please reach out via our [forum](https://discourse.canceridc.dev/) - we are very interested in hearing your feedback and suggestions!
{% endhint %}

IDC supports a variety of interfaces for fetching individual images, cohorts (groups of images), or portions of images, using desktop application, command-line interface, or programmatic API. These interfaces are covered in the subsequent pages. You should select the specific approach to accessing IDC data depending on your requirements.

* [idc-index](https://learn.canceridc.dev/data/downloading-data/downloading-data#command-line-or-programmatic-download-idc-index-python-package) interface: **command-line and Python API interface** to download images corresponding to the specific patient/study/series, or a cohort defined by a manifest
* [3D Slicer](downloading-data.md) interface: **desktop application** to download images corresponding to the specific patient/study/series, or a cohort defined by a manifest
* [s5cmd](downloading-data-with-s5cmd.md): command-line interface to download images for a cohort defined by a manifest (unlike `idc-index`, does not organize downloaded images into folders corresponding to IDC data model hierarchy)
* [DICOMweb](dicomweb-access.md) interface: **REST API interface** to access both metadata and pixel data at the granularity of image frames/tiles
* [Directly loading DICOM objects from Google Cloud or AWS in Python](direct-loading.md): **Python API interface** to access both metadata and pixel data at the granularity of image frames/tiles

