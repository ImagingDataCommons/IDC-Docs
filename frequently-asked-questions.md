# Frequently Asked Questions

### What is the purpose of IDC?

We list the core functions of IDC [here](core-functions-of-idc.md). Our overall purpose is to support cancer imaging research on the cloud. While some of the capabilities that are needed for this are outside the scope of IDC, and may necessitate interaction with the other components of the [Cancer Research Data Commons](https://datacommons.cancer.gov/), we welcome IDC users to bring up any use cases that are related to using cloud computing for cancer imaging research. We will work with you to identify relevant component of CRDC that can help you.

### What is the status of IDC?

IDC pilot release took place in Fall 2020. We anticipate the production version of IDC to become available in Spring 2021. You can learn about the planned milestones for the IDC development in [these slides presented at RSNA 2019](http://bit.ly/idc-rsna19).

### What data is available?

Our initial goal is to host all of the public collections from [The Cancer Imaging Archive \(TCIA\)](https://www.cancerimagingarchive.net/). The IDC pilot contains a subset of those collections. The complete up to date list of collections included in IDC is available [here](https://portal.imaging.datacommons.cancer.gov/collections/).

### How do I get my data into IDC?

For a dataset to become part of the IDC offering, it has to be de-identified and curated by TCIA, and released as [a public TCIA collection](https://www.cancerimagingarchive.net/collections/). Once this is done, it will \(eventually!\) be replicated in IDC.

### What is the difference between IDC and TCIA?

IDC and TCIA are partners in providing FAIR data for cancer imaging researchers. While some of the functions between the two resources are similar, there are also key differences. The table below provides a summary of similarities and differences.

| Function | **IDC** | TCIA |
| :--- | :--- | :--- |
| Source of FAIR data for cancer imaging research | yes | yes |
| Curation of cancer imaging collections | yes | yes |
| De-identification | no | yes |
| Cloud-based data co-located with compute resources | yes | no |
| Recommended mechanism for downloading data to a local resource | no | yes |
| Conversion of pathology images and image-derived data into DICOM format | yes | no |
| Cloud-based research use cases | yes | no |
| Private data collections | no | yes |
| Public data collections | yes | yes |

### Is IDC the right choice for my needs?

IDC is the right choice for you if you want to ...

* ... explore metadata, visualize images and annotations, build cohorts from the data included in public TCIA collections
* ... analyze TCIA public collections data on the cloud
* ... use existing tools such as Google Colab, BigQuery, DataStudio with the TCIA public collections data
* ... perform complex queries against any of the DICOM attributes in the TCIA public collections
* ... utilize other resources available in CRDC, such as CRDC Cloud Resources, for data analysis
* ... quickly visualize specific images from TCIA public collections

IDC is NOT the right choice for you if you want to ...

* ... download data to your computer
* ... upload and share publicly the imaging dataset you collected 
* ... de-identify your dataset

### What is the vision of the workflow for the user interacting with IDC?

1. Define relevant imaging data cohorts from public datasets \(based on rich standardized metadata\)
2. Explore the cohort, check quality \(visualize images and image-derived data\)
3. Apply off-the-shelf cloud analytics tools to the cohort
   1. BigQuery, DataStudio, Colab Notebooks
   2. Further cohort refinement, metadata exploration, analysis
4. Perform exploratory analysis of data on a cloud VM
5. Scale up analysis
6. Integrate with other data sources
   1. Private data
   2. Non-imaging data from CRDC \(i.e., genomics and proteomics\)
7. Share analysis results and workflows

### Where do I learn more about other components of CRDC?

The main website for the Cancer Research Data Commons \(CRDC\): [https://datacommons.cancer.gov/](https://datacommons.cancer.gov/).

### What about non-imaging data that accompanies many TCIA collections?

At the moment, non-imaging data, such as the spreadsheets with clinical information, is not replicated on IDC and it is not possible to search this data using IDC Portal. You will need to access this data from TCIA.

Our longer-term plan is to work with the CRDC [Center for Cancer Data Harmonization \(CCDH\)](https://datacommons.cancer.gov/center-cancer-data-harmonization) to harmonize the data in these spreadsheets, and identify the appropriate format and location for the resulting harmonized data. 

### Is it free to use IDC?

We provide a summary of what you can do with IDC, and what you will need to access specific capabilities [here](getting-started-with-idc.md). If you want to explore the capabilities of the cloud that require a billing account, and would like to develop a better understanding of the costs before committing your credit card, we have limited resources to support pilot projects exploring the use of IDC in cancer research. Please see details [here](introduction/requesting-gcp-cloud-credits.md).

