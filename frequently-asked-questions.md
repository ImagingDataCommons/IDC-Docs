# Frequently asked questions

## How to download data from IDC?

Check out the [downloading-data](data/downloading-data/ "mention") documentation page!

## How do I get my data into IDC?

Currently IDC will consider submissions from NCI-funded driving projects and data from special selected projects.

IDC only hosts public datasets. It does not support access limitations, such as data embargoes or sequestration.

Submitters of data to IDC must ensure that the data have been de-identified with respect to protected health information (PHI). Currently, IDC accepts data de-identified by [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/). Datasets de-identified by other data coordinating centers (DCC) may be considered but in such cases further information will be required.

If you are interested in making your data available within IDC, please contact us by sending email to [support+submissions@canceridc.dev](https://mail.google.com/mail/?view=cm\&fs=1\&tf=1\&to=support+submissions@canceridc.dev).

## How much does it cost to use the cloud?

You can search and [download data from IDC](data/downloading-data/) for free, and without having to attach your credit card to your GCP or AWS account.

You can use the free tier of [Google Colab](https://colab.research.google.com) to get free access to a cloud-hosted VM with GPU to experiment with analysis workflows for IDC data.

We provide a [summary](getting-started-with-idc.md) of what you can do with IDC, and what you will need to access specific capabilities. If you want to explore the capabilities of the cloud that require a billing account, and would like to develop a better understanding of the costs before committing your credit card, you can apply for a free Google cloud credit allocation using [this form](https://docs.google.com/forms/d/e/1FAIpQLSfXvXqficGaVEalJI3ym6rKqarmW\_YUUWG6A4U8pclvR8MmRQ/viewform).

## What is the purpose of IDC?

IDC is a cloud-based repository of publicly available cancer imaging data co-located with analysis and exploration tools and resources.

First, we want to enable you to accomplish a lot of tasks related to data exploration (search, visualization, subsetting, cohort building) without having to download anything to your computer. We accomplish this by co-locating data with the cloud-based tools that support those tasks.

Once you identify data of interest, you can either download it to your computer and proceed with your analysis workflows, or you can cross-load the data to a cloud-based VM for further analysis. The latter should hopefully make it easier to reproduce and share your analysis workflow.

## What is the status of IDC?

IDC pilot release took place in Fall 2020, followed by the production release in September 2021. You can learn about the planned milestones for the IDC development in [these slides presented at RSNA 2019](http://bit.ly/idc-rsna19).

## What data is available?

We host most of the public collections from [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net). We also host HTAN and other pathology images not hosted by TCIA. You can review the complete, up-to-date list of [collections included in IDC](https://portal.imaging.datacommons.cancer.gov/collections/).

## How to acknowledge IDC?

Please cite the paper below:

> Fedorov, A., Longabaugh, W. J. R., Pot, D., Clunie, D. A., Pieper, S., Aerts, H. J. W. L., Homeyer, A., Lewis, R., Akbarzadeh, A., Bontempi, D., Clifford, W., Herrmann, M. D., Höfener, H., Octaviano, I., Osborne, C., Paquette, S., Petts, J., Punzo, D., Reyes, M., Schacherer, D. P., Tian, M., White, G., Ziegler, E., Shmulevich, I., Pihl, T., Wagner, U., Farahani, K. & Kikinis, R. NCI Imaging Data Commons. Cancer Res. **81,** 4188–4193 (2021). [http://dx.doi.org/10.1158/0008-5472.CAN-21-0950](http://dx.doi.org/10.1158/0008-5472.CAN-21-0950)

## What is the difference between IDC and TCIA?

IDC and TCIA are partners in providing FAIR data for cancer imaging researchers. While some of the functions between the two resources are similar, there are also key differences. The table below provides a summary of similarities and differences.

| Function                                                                | **IDC**                        | TCIA |
| ----------------------------------------------------------------------- | ------------------------------ | ---- |
| Source of FAIR data for cancer imaging research                         | yes                            | yes  |
| Curation of cancer imaging collections                                  | yes                            | yes  |
| De-identification (note: IDC hosts data de-identified by TCIA)          | no                             | yes  |
| Cloud-based data co-located with compute resources                      | yes                            | no   |
| Conversion of pathology images and image-derived data into DICOM format | yes                            | no   |
| Cloud-based research use cases                                          | yes                            | no   |
| Private data collections                                                | no                             | yes  |
| Public data collections                                                 | yes                            | yes  |
| Version control of the data                                             | [yes](data/data-versioning.md) | no   |

## Is IDC the right choice for my needs?

IDC is the right choice for you if you want to...

* Explore metadata, visualize images and annotations, build cohorts from the data included in public TCIA collections
* Analyze TCIA public collections data on the cloud
* Use existing tools such as Google Colab, BigQuery, and DataStudio with the TCIA public collections data
* Perform complex queries against any of the DICOM attributes in the TCIA public collections
* Utilize other resources available in CRDC, such as CRDC Cloud Resources, for data analysis
* Quickly visualize specific images from TCIA public collections

IDC is NOT the right choice for you if you want to...

* Have your dataset de-identified by IDC

## What is the vision of the workflow for the user interacting with IDC?

1. Define relevant imaging data cohorts from public datasets (based on rich standardized metadata).
2. Explore the cohort, check quality (visualize images and image-derived data).
3. Apply off-the-shelf cloud analytics tools to the cohort:
   * BigQuery, DataStudio, Colab Notebooks
   * Further cohort refinement, metadata exploration, analysis
4. Perform exploratory analysis of data on a cloud VM.
5. Scale-up analysis.
6. Integrate with other data sources:
   * Private data
   * Non-imaging data from CRDC (i.e., genomics and proteomics)
7. Share analysis results and workflows.

## Where do I learn more about other components of CRDC?

The main website for the Cancer Research Data Commons (CRDC) is [https://datacommons.cancer.gov/](https://datacommons.cancer.gov)

## What about non-imaging data that accompanies many TCIA collections?

Clinical data that was shared by the submitters is available in a dedicated BigQuery dataset. Please see [this tutorial](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/notebooks/clinical\_data\_intro.ipynb) on how to search that data and how to link clinical data with imaging metadata!

## I want to search IDC content using an attribute not available in the portal

Check out our [Getting Started tutorial series](https://github.com/ImagingDataCommons/IDC-Tutorials/tree/master/notebooks/getting\_started) to learn how to use SQL for searching IDC data.&#x20;

If you are not comfortable writing queries, you can use [this DataStudio dashboard](https://datastudio.google.com/reporting/ab96379c-e134-414f-8996-188e678f1b70/page/KHtxB) to search using some of the attributes that are not available through the portal. You can also [extend this dashboard](cookbook/data-studio/cohort-dashboard.md) to include additional attributes.
