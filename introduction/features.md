# Features

The following components and capabilities are being developed by IDC:

* [Cloud-hosted imaging data collections](../data/introduction.md)
* [Search and cohort building portal](../portal/getting-started.md) \(IDC Portal\)
* Visualization of the hosted imaging data supported by the integrated [OHIF Viewer](https://github.com/OHIF/Viewers)
* [Application Programming Interface \(API\)](../api/getting-started.md) to support programmatic use of the IDC functionality

IDC is being built utilizing the Google Cloud Platform \(GCP\) as the foundation. We use a range of products developed by GCP, most notably Google Healthcare, to implement the capabilities listed above. We discuss the role of the major GCP components used by IDC in [this section](google-cloud-platform.md).

IDC is relying on the [Digital Imaging and Communications in Medicine \(DICOM\)](https://www.dicomstandard.org/) standard for data modeling and representation. Most of the data hosted by IDC is in DICOM format. Our goal is to have all of the imaging \(including digital pathology, microscopy and other non-radiological image types\) and image-derived data \(such as results of segmentation, image annotation, or post-processing\) represented in the DICOM format. You can read more about our motivation and details of our approach in [this section](dicom.md).

