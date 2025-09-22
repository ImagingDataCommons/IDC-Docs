# Frequently asked questions

## What is the difference between IDC and TCIA?

IDC and TCIA are partners in providing FAIR data for cancer imaging researchers.&#x20;

TCIA provides unique service to work with data submitters to de-identify cancer imaging data and make it available for download.

The mission of IDC is to support efficient access and use of the cancer imaging data, after it was de-identified and released. Some of the highlights that make IDC unique are the following:

* while all of the public TCIA DICOM collections are available in IDC, there is a growing amount of data in IDC that is not available anywhere else:
  * DICOM digital pathology collections from prominent initiatives: Childhood Cancer Data Initiative (CCDI), GTEx, TCGA, CPTAC, HTAN, CMB
  * image analysis results available only from IDC, such as TotalSegmentator segmentations and radiomics features for most of the CT images in the NLST collection
* IDC makes the data available in public cloud buckets, the egress is free (TCIA provides download from on-premises servers at a single institution): chances are your will be able to download data from IDC much faster than from TCIA
* IDC maintains superior community recognized tools to support the use of the data:&#x20;
  * modern OHIF Viewer v3 for radiology data, with support of visualization of annotations and segmentations;&#x20;
  * Slim viewer for digital pathology and annotations
  * highly capable IDC Portal
* IDC offers standard interfaces for data access: S3 API for file download, DICOMweb for interoperability with DICOM tools, SQL for searching all of the DICOM metadata (TCIA offers various non-standard, in-house interfaces and APIs for data access)
* All of the data (radiology and digital pathology images, annotations, segmentations, image-derived features) available in IDC is harmonized into DICOM representation, which means&#x20;
  * interoperability: you can use IDC data with any DICOM-compatible tool
  * metadata: every single file in IDC is accompanied by metadata that follows DICOM data model, and is associated with unique identifiers, allowing you to build reproducible cohorts
  * uniform representation: you don't need to customize your processing pipelines to a specific collection, and can build cohorts combining data across collections
* IDC data is easier to access from cloud computing resources, allowing you to more easily experiment with the new analysis tools and scale your computation
* IDC data is versioned: you will be able to access the exact files you analyzed in a given verison of IDC even if there were any updates to the collection after you accessed it, helping you achieve reproducibility of your analyses

## How to download data from IDC?

Check out the [downloading-data](data/downloading-data/ "mention") documentation page!

## How do I get my data into IDC?

Note that currently IDC prioritizes submissions from NCI-funded driving projects and data from special selected projects.

* If you would like to submit images, it will be your responsibility to de-identify them first, documenting the de-identification process and submitting that documentation for the review by IDC stakeholders.
* We welcome submissions of image-derived data (expert annotations, AI-generated segmentations) for the images already in IDC, see IDC Zenodo community [Curation policy](https://zenodo.org/communities/nci-idc/curation-policy) to learn about the requirements for such submissions!

IDC works closely with  [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/) and mirrors TCIA public collections. If you submit your DICOM data to TCIA and your data is released as a public collection, it will be automatically available in IDC in a following release.

If you are interested in making your data available within IDC, please contact us by sending email to [support+submissions@canceridc.dev](https://mail.google.com/mail/?view=cm\&fs=1\&tf=1\&to=support+submissions@canceridc.dev).

## How much does it cost to use the cloud?

IDC data is stored in the cloud buckets, and you can search and [download data from IDC](data/downloading-data/) for free and without login.

If you would like to use the cloud for analysis of the data, we recommend you start with the free tier of [Google Colab](https://colab.research.google.com) to get free access to a cloud-hosted VM with GPU to experiment with analysis workflows for IDC data. If you are an NIH-funded researcher, you may be eligible for a free allocation via [NIH Cloud Lab](https://cloud.nih.gov/resources/cloudlab/). US-based researchers can also access free cloud-based computing resources via [ACCESS program allocations](cookbook/access-allocations.md).

## What is the status of IDC?

IDC pilot release took place in Fall 2020, followed by the production release in September 2021. IDC team is continuously refining the capabilities of IDC Portal and various tools, and publishes new data releases every 3-4 months.

## What data is available?

We host most of the public collections from [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net). We also host HTAN and other pathology images not hosted by TCIA. You can review the complete, up-to-date list of [collections included in IDC](https://portal.imaging.datacommons.cancer.gov/collections/).

## How to acknowledge IDC?

Please cite the latest paper from the IDC team. Please also make sure you acknowledge the specific data collections you used in your analysis.

> Fedorov, A., Longabaugh, W. J. R., Pot, D., Clunie, D. A., Pieper, S. D., Gibbs, D. L., Bridge, C., Herrmann, M. D., Homeyer, A., Lewis, R., Aerts, H. J. W. L., Krishnaswamy, D., Thiriveedhi, V. K., Ciausu, C., Schacherer, D. P., Bontempi, D., Pihl, T., Wagner, U., Farahani, K., Kim, E. & Kikinis, R. National cancer institute imaging data commons: Toward transparency, reproducibility, and scalability in imaging artificial intelligence. _Radiographics_ 43, (2023). [https://doi.org/10.1148/rg.230180](https://doi.org/10.1148/rg.230180)

## Where do I learn more about other components of CRDC?

The main website for the Cancer Research Data Commons (CRDC) is [https://datacommons.cancer.gov/](https://datacommons.cancer.gov)

## What about non-imaging data that accompanies IDC collections?

Clinical data that was shared by the submitters is available for a number of imaging collections in IDC. Please see [this tutorial](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/advanced_topics/clinical_data_intro.ipynb) on how to search that data and how to link clinical data with imaging metadata!

Many of the imaging collections are also accompanied by the genomics or proteomics data. CRDC [Cancer Data Aggregator (CDA)](https://cda.readthedocs.io/en/latest/) provides the API to locate such related datasets.

## I want to search IDC content using an attribute not available in the portal

IDC Portal gives you access to just a small subset of the metadata accompanying IDC images. If you want to learn more about what is available, you have several options:

* [this notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting_started/part2_searching_basics.ipynb) from our Getting Started tutorial series explains how to use [`idc-index`](https://github.com/ImagingDataCommons/idc-index) - a python package that aims to simplify access to IDC data
* [this more advanced notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting_started/part3_exploring_cohorts.ipynb) will help you get started with searching IDC metadata in BigQuery, which gives you access to all of the DICOM metadata extracted from IDC-hosted files
* if you are not comfortable writing queries or coding in pyhon, you can use [this DataStudio dashboard](https://datastudio.google.com/reporting/ab96379c-e134-414f-8996-188e678f1b70/page/KHtxB) to search using some of the attributes that are not available through the portal. You can also [extend this dashboard](cookbook/data-studio/cohort-dashboard.md) to include additional attributes.
