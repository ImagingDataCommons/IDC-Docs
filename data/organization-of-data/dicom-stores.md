# DICOM stores

If you would like to access IDC data via [DICOMweb interface](https://www.dicomstandard.org/using/dicomweb), you have two options:&#x20;

1. IDC-maintained DICOM store available via proxy
2. DICOM store maintained by Google Healthcare

In the following we provide details for each of those options.

### IDC-maintained DICOM store via proxy

This store contains all of the data for the current IDC data release. It does not require authentication. and is available via the following DICOMweb URL of the proxy (you can ignore the "viewer-only-no-downloads" part in the URL, it is a legacy constraint that is no longer applicable).

DICOMweb URL:

{% code overflow="wrap" %}
```
https://proxy.imaging.datacommons.cancer.gov/current/viewer-only-no-downloads-see-tinyurl-dot-com-slash-3j3d9jyp/dicomWeb
```
{% endcode %}

**Limitations**:

* since all requests go through the proxy, you may experience reduced performance as compared to direct access you can achieve using the store described in the following section
* there are per-IP and overall daily quotas, as described in IDC [Proxy policy](../../portal/proxy-policy.md), that may not be sufficient for your use case

### DICOM store maintained by Google Healthcare

This store replicates all of the data from the `idc-open-data` bucket (see related discussion on why IDC uses multiple cloud buckets to organize the data in [this forum thread](https://discourse.canceridc.dev/t/what-is-the-difference-between-the-three-aws-buckets/459)), which is limited to the data available without commercial use constraints (i.e., under CC-BY license), and does not have the data that was flagged by TCIA as potentially containing patient faces.&#x20;

DICOMweb URL:

{% code overflow="wrap" %}
```
https://healthcare.googleapis.com/v1/projects/nci-idc-data/locations/us-central1/datasets/idc/dicomStores/idc-store-v20/dicomWeb
```
{% endcode %}

This DICOM store is documented in [https://cloud.google.com/healthcare-api/docs/resources/public-datasets/idc](https://cloud.google.com/healthcare-api/docs/resources/public-datasets/idc).&#x20;

{% hint style="danger" %}
As of Jul 1, 2025, [Google documentation page](https://cloud.google.com/healthcare-api/docs/resources/public-datasets/idc) incorrectly instructs users to request access in order to be whitelisted and able to access the store. In reality, anyone logged in with a Google account is able to access the store!
{% endhint %}

**Limitations**:

* most, but not all of the IDC data is available in this store
* authentication is required

## DICOMweb usage tutorials

We do not yet have a dedicated IDC tutorial on this topic. In the meantime, you can consult the Google Healthcare documentation page for instructions: [https://cloud.google.com/healthcare-api/docs/how-tos/dicomweb](https://cloud.google.com/healthcare-api/docs/how-tos/dicomweb), or use any other tutorial/resource on the use of DICOMweb!
