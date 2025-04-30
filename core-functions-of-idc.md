# Core functions

## Easy and efficient access to public cancer imaging data

We ingest and distribute datasets from variety of sources and contributors, primarily focusing on large data collection initiatives sponsored by US National Cancer Institute.&#x20;

At this time, we do not have resources to prioritize receipt of the imaging data from individual PIs (but we are encouraging submissions of annotations/analysis results for existing IDC data!). Nevertheless, if you feel you might have a compelling dataset, please email us at [support+submissions@canceridc.dev](https://mail.google.com/mail/?view=cm\&fs=1\&tf=1\&to=support+submissions@canceridc.dev).

On ingestion, we harmonize images and image-derived data into DICOM format for interoperability, whenever data is represented in a non-DICOM format.

Upon conversion, the data undergoes Extract-Transform-Load (ETL), which extracts DICOM metadata to make the data searchable, ingests the DICOM files into public S3 storage buckets and a DICOMweb store. Once the data is released, we provide various interfaces to access data and metadata.

{% embed url="https://docs.google.com/presentation/d/1UVpNVyVy3xIYLDnm4rtgAUmSu-uKQo5krekI9DSMT8o/edit#slide=id.p" %}
Schematic summary of the IDC data ingestion and release process.
{% endembed %}

## Tools to simplify the use of the data

We are actively developing a variety of capabilities to make it easier for the users to work with the data in IDC. Some of the examples of those tools include

* [IDC Portal](https://portal.imaging.datacommons.cancer.gov/explore/) provides interactive browser-based interface for exploration of IDC data
* we are the maintainers of [Slim](https://github.com/ImagingDataCommons/slim) - an open-source viewer of DICOM digital pathology images; Slim is integrated with IDC Portal for visualizing pathology images and image-derived data available in IDC
* we are actively contributing to the [OHIF Viewer](https://github.com/OHIF/Viewers), and rely on it for visualizing radiology images and image-derived data
* [`idc-index`](https://github.com/ImagingDataCommons/idc-index) is a python package that provides convenience functions for accessing IDC data, including efficient download from IDC public S3 buckets
* [3D Slicer](https://slicer.org) extensions [SlicerIDCBrowser](https://github.com/ImagingDataCommons/SlicerIDCBrowser) can be used for interactive download of IDC data
* we are contributing to a variety of tools that aim to simplify the use of DICOM in cancer imaging research; these include [OpenSlide](https://openslide.org/formats/dicom/) and [BioFormats bfconvert](https://bio-formats.readthedocs.io/en/v7.3.1/formats/dicom.html) library that can be used for conversion between DICOM Whole Slide Imaging (WSI) format and other slide microscopy formats, [dcmqi](https://github.com/QIICR/dcmqi) library for converting image analysis results to and from DICOM representation

{% embed url="https://docs.google.com/presentation/d/13NQKWfauODArO4A6BrxJNZQODKoq5tJEQ0xg_Dvji7s/edit?usp=sharing" %}
Although IDC data is stored in DICOM format, it can be converted into alternative research representations using open-source tools.&#x20;
{% endembed %}

## Support of continuous enrichment of data

We welcome you to apply to contribute analysis results and annotations of the images available in IDC! These can be expert manual annotations, analysis results generated using AI tools, segmentations, contours, metadata attributes describing the data (e.g., annotation of the scan type), expert evaluation of the quality of existing AI-generated annotations in IDC.

If you would like your annotations/analysis results to be considered, you must establish the value of your contribution (e.g., describe the qualifications of the experts performing manual annotations, demonstrate robustness of the AI tool you are applying to images with a peer-reviewed publication or other type of evidence), and be willing to share your contribution under a permissive Creative Commons Attribution [CC BY 4.0 license](https://creativecommons.org/licenses/by/4.0/deed.en).&#x20;

See more details on our curation policy [here](https://zenodo.org/communities/nci-idc/curation-policy), and reach out by sending email to [support+submissions@canceridc.dev](https://mail.google.com/mail/?view=cm\&fs=1\&tf=1\&to=support+submissions@canceridc.dev) with any questions or inquries. Every application will be reviewed by IDC stakeholders.

If your contribution is accepted by the IDC stakeholders:

* we will work with you to choose the appropriate DICOM object type for your data and convert it into DICOM representation
* upon conversion, we will create a Zenodo entry under the[NCI Imaging Data Commons Zenodo community](https://zenodo.org/communities/nci-idc/) for your contribution so that you get the Digital Object Identifier (DOI), citation and recognition of your contribution
* once published in IDC
  * your data will become searchable and viewable in IDC Portal, so it is easier for the users of your data to discover and work with your data
  * files can be downloaded very efficiently using S3 interface and `idc-index`

## Integration of cancer imaging data with other components of CRDC

IDC is a component of the broader NCI [Cancer Research Data Commons (CRDC)](https://datacommons.cancer.gov/), giving you access to the following:

* [Cancer Data Aggregator (CDA)](https://cda.readthedocs.io/en/latest/) can be used to find data related to the images in IDC in [Genomics Data Commons](https://portal.gdc.cancer.gov/), [Proteomics Data Commons](https://pdc.cancer.gov/pdc/) and [Integrated Canine Data Commons](https://caninecommons.cancer.gov/)
* Broad [FireCloud](https://firecloud.terra.bio/) and [Seven Bridges Cancer Genimics Cloud](https://www.cancergenomicscloud.org/) (SB-CGC) can be used to apply analysis tools to the data in IDC (you can read more about how this can be done in [this preprint](https://doi.org/10.21203/rs.3.rs-4351526/v1) from the IDC team)
* [MHub.AI](https://mhub.ai/) platform curates a growing number of cancer imaging AI models that can be applied directly to the DICOM data available in IDC

