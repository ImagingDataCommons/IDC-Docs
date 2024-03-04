# Proxy policy

{% hint style="info" %}
TL;DR: if you want to download images from IDC, you can do it without charge, limits or sign-ins from our cloud storage buckets. See instructions in [this article](../data/downloading-data/).
{% endhint %}

The primary mechanism for accessing data from IDC is by searching the metadata using the idc-index python package or BigQuery tables, and downloading the binary files from public cloud buckets, as discussed in [this article](https://app.gitbook.com/o/-MCNUE8xpdPUJwWRz6xu/s/-MCTG4fXybYgGMalZnmf-2668963341/data/downloading-data). There is no limit, quota or fees associated with downloading IDC files from the buckets.

Effective March 2024, as a pilot project, IDC also provides access to the DICOM data via the DICOMweb interface available at this endpoint: [https://proxy.imaging.datacommons.cancer.gov/current/viewer-only-no-downloads-see-tinyurl-dot-com-slash-3j3d9jyp/dicomWeb](https://proxy.imaging.datacommons.cancer.gov/current/viewer-only-no-downloads-see-tinyurl-dot-com-slash-3j3d9jyp/dicomWeb). This endpoint is read-only. It will route the requests to the Google Healthcare API DICOM store containing IDC data.

Our DICOMWeb endpoint should only be used when data access needs cannot be satisfied using other mechanisms (e.g., when accessing individual frames of the microscopy images without having to download the entire binary file).&#x20;

Egress of data via the DICOMweb interface is capped at a non-disclosed limit that is tracked per IP. It is not acceptable to “IP hop” in an attempt to circumvent individual daily quotas, since there is also a global daily cap as well to prevent full egress of the imaging collection. Note that if this global cap is hit, all other users of the site would be unable to use the viewers for the rest of the day (using the UTC clock). Thus, IP hopping against the proxy that causes the global quota to be hit will be considered a denial-of-service attack.

If you reach your daily quota, but feel you have a compelling cancer imaging research use case to request an exception to the policy and an increase in your daily quota, please reach out to us at support@canceridc.dev to discuss the situation.

We are continuously monitoring the usage of the proxy. Depending on the actual costs and usage, this policy may be revisited in the future to restrict access via the DICOMweb interface for any uses other than IDC viewers.

\
