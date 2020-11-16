# Data Known Issues



1. Indexing of the collection of [NSCLC-Radiomics](https://wiki.cancerimagingarchive.net/display/Public/NSCLC-Radiomics)  by the Data Commons Framework is pending.
2. [QIN multi-site collection of Lung CT data with Nodule Segmentations](https://doi.org/10.7937/K9/TCIA.2015.1BUVFJR7): only items corresponding to the LIDC-IDRI original collection are included
3. [DICOM SR of clinical data and measurement for breast cancer collections to TCIA](https://doi.org/10.7937/TCIA.2019.wgllssg1): only items corresponding to the ISPY1 original collection are included
4. [ISPY1 \(ACRIN 6657\)](https://doi.org/10.7937/K9/TCIA.2016.HdHpgJLK): Some of the segmentations in this collection are empty \(as an example, SeriesNumber 42100 with SeriesDescription "VOI PE Segmentation thresh=70" in [this study](https://viewer.imaging.datacommons.cancer.gov/viewer/1.3.6.1.4.1.14519.5.2.1.7695.1700.186130315530827043916665138479) is empty\).
5. Due to the existing limitations of Google Healthcare API, not all of the DICOM attributes are extracted and are available in BigQuery tables. Specifically:
   * sequences that have more than 15 levels of nesting are not extracted \(see [https://cloud.google.com/bigquery/docs/nested-repeated](https://cloud.google.com/bigquery/docs/nested-repeated)\) - we believe this limitation does not affect the data stored in IDC
   * sequences that contain around 1MiB of data are dropped from BigQuery export and RetrieveMetadata output currently. 1MiB is not an exact limit, but it can be used as a rough estimate of whether or not the API will drop the tag \(this limitation was not documented as of writing this\) - we know that some of the instances in IDC will be affected by this limitation. The fix for this limitation is targeted for sometime in 2021, according to the communication with Google Healthcare support. 

