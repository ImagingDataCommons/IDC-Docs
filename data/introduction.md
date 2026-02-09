# Introduction

## Data sources

Most of the data in IDC is received from the data collection initiatives/projects supported by US National Cancer Institute. Whenever source images or image-derived data is not in the DICOM format, it is harmonized into DICOM as part of the ingestion.&#x20;

As of data release v23, IDC sources of data include:

* [The Cancer Imaging Archive (TCIA) (ongoing)](https://www.cancerimagingarchive.net/)
  * all DICOM files from the public collections are mirrored in IDC
  * a subset of digital pathology collections and analysis results harmonized from vendor-specific representation (as available from TCIA) into DICOM Slide Microscopy (SM) format&#x20;
* [Childhood Cancer Data Initiative (CCDI) (ongoing)](https://www.cancer.gov/research/areas/childhood/childhood-cancer-data-initiative)
  * digital pathology slides harmonized into DICOM SM
* [Genomic Data Commons (GDC)](https://portal.gdc.cancer.gov/)
  * The Cancer Genome Atlas (TCGA) slides harmonized into DICOM SM
* [Human Tumor Atlas Network (HTAN)](https://humantumoratlas.org/)
  * release 1 of the HTAN data harmonized into DICOM SM
* [National Library of Medicine Visible Human Project](https://www.nlm.nih.gov/research/visible/visible_human.html)
  * v1 of the Visible Human images harmonized into DICOM MR/CT/XC
* [Genotype-Tissue Expression Project (GTex)](https://commonfund.nih.gov/GTEx)
  * digital pathology slides harmonized into DICOM SM
* [BoneMarrowWSI-PediatricLeukemia](https://doi.org/10.5281/zenodo.14933087)
  * a comprehensive dataset of bone marrow aspirate smear whole slide images with expert annotations and clinical data in pediatric leukemia

The list of all of the IDC collections is available in IDC Portal here: [https://portal.imaging.datacommons.cancer.gov/collections/](https://portal.imaging.datacommons.cancer.gov/collections/).

## Data provenance

Whenever IDC replicates data from a publicly available source, we include the reference to the origin:

* from the IDC Portal Explore page, click on the "i" icon next to the collection in the collections list&#x20;

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

* `source_doi` metadata column contains Digital Object Identifier (DOI) at the granularity of the individual files and is available both via [python `idc-index` package](https://github.com/ImagingDataCommons/idc-index) (see [this tutorial](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting_started/part2_searching_basics.ipynb) on how to access it) and BigQuery interfaces

{% hint style="info" %}
Whenever source data is harmonized into DICOM, the DOI will correspond to a Zenodo entry for the result of harmonization, which in turn will reference the location where data can be accessed in the native format (if available). As an example, IDC NLM-Visible-Human-Project collection refers to this DOI that describes the dataset resulting from the original dataset harmonized into DICOM [https://doi.org/10.5281/zenodo.12690049](https://doi.org/10.5281/zenodo.12690049), which in turn references the [NLM Visible Human project page](https://www.nlm.nih.gov/research/visible/visible_human.html) containing information on accessing the original files collected by the project.&#x20;
{% endhint %}

Check out [Data release notes](data-release-notes.md) for information about the collections added in the individual IDC data releases.

## Data ingestion process

Simplified workflow for IDC data ingestion is summarized in the following diagram.

{% embed url="https://docs.google.com/presentation/d/1UVpNVyVy3xIYLDnm4rtgAUmSu-uKQo5krekI9DSMT8o/edit?slide=id.g2fbbb94d529_0_76#slide=id.g2fbbb94d529_0_76" %}
IDC data ingestion workflow
{% endembed %}

