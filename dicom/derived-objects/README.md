# Derived objects

In this section we discuss derived DICOM objects, including annotations, that are stored in IDC. It is important to recognize that in practice annotations are often shared in non-standard formats. When IDC ingests a dataset where annotations are available in such a non-standard representation,  those need to be harmonized into a suitable DICOM object to be available in IDC. Due to the complexity of this task, we are unable to perform such harmonization for all of the datasets. If you want to check if there are annotations in non-DICOM format available for a given collection, you should locate the original source of the data, and examine the accompanying documentation for available non-DICOM annotations.

As an example, [Breast-Cancer-Screening-DBT](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=breast\_cancer\_screening\_dbt) collection is available in IDC. If you mouse over the name of that collection in the IDC Portal, the tooltip will provide the overview of the collection and the link to the source.

![](<../../.gitbook/assets/image (2).png>)

You will also find the link to the source in the [list of collections available in IDC](https://portal.imaging.datacommons.cancer.gov/collections/).

![](../../.gitbook/assets/image.png)

Finally, if you select data using SQL, you can use the `source_DOI` column to identify the source of each file in the subset you selected (lean more about `source_DOI`, licenses and attribution in the part 3 of our [Getting started tutorial](https://github.com/ImagingDataCommons/IDC-Tutorials/tree/master/notebooks/getting\_started)).

For the collection in question, the source DOI is [https://doi.org/10.7937/e4wt-cd02](https://doi.org/10.7937/e4wt-cd02), and examining that page you will see a pointer to the CSV file with the coordinates of the bounding boxes defining regions containing lesions.

Non-standard annotations are not searchable, usually are not possible to visualize in off-the-shelf tools, and require custom code to interpret and parse. The situation is different for the DICOM derived objects that we discuss in the following sections.

### DICOM derived objects

In IDC we define "derived" DICOM objects as those that are obtained by analyzing or post-processing the "original" image objects. Examples of derived objects can be annotations of the images to define image regions, or to describe findings about those regions, or voxel-wise parametric maps calculated for the original images.

Although DICOM standard provides a variety of mechanisms that can be used to store specific types of derived objects, most of the image-derived objects currently stored in IDC fall into the following categories:

* voxel segmentations stored as [DICOM Segmentation objects (SEG)](dicom-segmentations.md)
* segmentations defined as a set of planar regions stored as [DICOM Radiotherapy Structure Set objects (RTSTRUCT)](./#radiotherapy-structure-sets)
* quantitative measurements and qualitative evaluations for the regions defined by DICOM Segmentations, those will be stored as a specific type of [DICOM Structured Reporting (SR)](dicom-structured-reports.md) objects that follows DICOM SR template [TID 1500 "Measurements report"](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter\_A.html#sect\_TID\_1500) (SR-TID1500)

The type of the object is defined by the object class unique identifier stored in the `SOPClassUID` attribute of each DICOM object. In the IDC Portal we allow the user to define the search filter based on the human-readable name of the class instead of the value of that identifier.

You can find detailed descriptions of these objects applied to specific datasets in TICA in the following open access publications:

> Fedorov, A., Clunie, D., Ulrich, E., Bauer, C., Wahle, A., Brown, B., Onken, M., Riesmeier, J., Pieper, S., Kikinis, R., Buatti, J. & Beichel, R. R. DICOM for quantitative imaging biomarker development: a standards based approach to sharing clinical data and structured PET/CT analysis results in head and neck cancer research. PeerJ **4,** e2057 (2016). [https://peerj.com/articles/2057/](https://peerj.com/articles/2057/)
>
> Fedorov, A., Hancock, M., Clunie, D., Brochhausen, M., Bona, J., Kirby, J., Freymann, J., Pieper, S., J W L Aerts, H., Kikinis, R. & Prior, F. DICOM re-encoding of volumetrically annotated Lung Imaging Database Consortium (LIDC) nodules. Med. Phys. (2020). [doi:10.1002/mp.14445](https://pubmed.ncbi.nlm.nih.gov/32772385/)



