# Features

The following components and capabilities are being developed by IDC:

* [Cloud-hosted imaging data collections](../data/introduction.md)
* [Search and cohort building portal](../portal/getting-started.md) (IDC Portal)
* Visualization of the hosted imaging data supported by the integrated [OHIF Viewer](https://github.com/OHIF/Viewers) (radiology data) and [SliM viewer](https://github.com/MGHComputationalPathology/slim) (digital pathology data)
* [Application Programming Interface (API)](../api/getting-started.md) to support programmatic use of the IDC functionality
* [Interactive self-guided tutorials](https://github.com/ImagingDataCommons/IDC-Tutorials/tree/master/notebooks/getting\_started) to help you get started

IDC is being built utilizing the Google Cloud Platform (GCP) as the foundation. We use a range of products developed by GCP, most notably Google Healthcare, to implement the capabilities listed above. We discuss the role of the major GCP components used by IDC in [this section](google-cloud-platform/).

IDC data in files is hosted both in Google Cloud Storage (GCS) buckets and (starting from v14) in Amazon AWS S3 buckets. GCS data storage and egress are sponsored by [Google Public Data Program](https://console.cloud.google.com/marketplace/product/bigquery-public-data/nci-idc-data). AWS data storage and egress sponsored by [AWS Open Data Sponsorship Program](https://registry.opendata.aws/nci-imaging-data-commons/).

{% hint style="success" %}
You can download IDC data (either to your computer or to a cloud VM) from either GCS or AWS buckets without the need to login, and without having to pay for egress!
{% endhint %}

IDC is relying on the [Digital Imaging and Communications in Medicine (DICOM)](https://www.dicomstandard.org/) standard for data modeling and representation. Most of the data hosted by IDC is in DICOM format. Our goal is to have all of the imaging (including digital pathology, microscopy and other non-radiological image types) and image-derived data (such as results of segmentation, image annotation, or post-processing) represented in the DICOM format. You can read more about our motivation and details of our approach in [this section](dicom.md).
