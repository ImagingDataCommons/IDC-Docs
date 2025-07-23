# DICOM stores

If you would like to access IDC data via [DICOMweb interface](https://www.dicomstandard.org/using/dicomweb), you have two options:&#x20;

1. IDC-maintained DICOM store available via proxy
2. DICOM store maintained by Google Healthcare

In the following we provide details for each of those options.

### IDC-maintained DICOM store via proxy

This store contains all of the data for the current IDC data release. It does not require authentication and is available via the following DICOMweb URL of the proxy (you can ignore the "viewer-only-no-downloads" part in the URL, it is a legacy constraint that is no longer applicable).

DICOMweb URL:

{% code overflow="wrap" %}
```
https://proxy.imaging.datacommons.cancer.gov/current/viewer-only-no-downloads-see-tinyurl-dot-com-slash-3j3d9jyp/dicomWeb
```
{% endcode %}

**Limitations**:

* since all requests go through the proxy before reaching the DICOM store, you may experience reduced performance as compared to direct access you can achieve using the store described in the following section
* there are per-IP and overall daily quotas, as described in IDC [Proxy policy](../../portal/proxy-policy.md), that may not be sufficient for your use case

### DICOM store maintained by Google Healthcare

This store replicates all of the data from the `idc-open-data` bucket, which contains most of the data in IDC (learn more about the organization of data in IDC buckets from [this documentation article](files-and-metadata.md#storage-buckets)).&#x20;

DICOMweb URL (note the store name includes the IDC data release version that corresponds to its content: `idc-store-v21`):

{% code overflow="wrap" %}
```
https://healthcare.googleapis.com/v1/projects/nci-idc-data/locations/us-central1/datasets/idc/dicomStores/idc-store-v21/dicomWeb
```
{% endcode %}

This DICOM store is documented in [https://cloud.google.com/healthcare-api/docs/resources/public-datasets/idc](https://cloud.google.com/healthcare-api/docs/resources/public-datasets/idc).&#x20;

**Limitations**:

* most, but not all of the IDC data is available in this store
* authentication with a Google account is required (anyone signed in with a Google account can access this interface, no whitelisting is required!)
* since this DICOM store is not maintained directly by the IDC team, it may lag behind the latest IDC release in content in the future

## DICOMweb usage tutorials

Check out [this tutorial](../downloading-data/dicomweb-access.md) and the accompanying Colab notebook to learn more.
