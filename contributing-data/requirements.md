# Requirements

IDC stakeholders decide which contributions are accepted. They are the IDC team leads, project managers and federal leads. Every contribution is reviewed individually, and acceptance is not guaranteed.

## Public access and license

IDC only hosts public data. Your data must be available to anyone without a data use or data transfer agreement.

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) is strongly preferred. Non-commercial licenses are strongly discouraged and are considered only under exceptional circumstances. See [Licensing and attribution](../data/licensing.md) for how licenses apply to IDC data.

Why CC BY:

* **It lets everyone use the data.** IDC's goal is to accelerate scientific discovery and the search for cancer cures. Academic and commercial researchers both do that work, and a non-commercial (NC) license excludes the commercial ones.
* **NC licenses leave model training in doubt.** It is not settled whether a model trained on NC data is itself bound by the NC clause. There is no established precedent, and it has been argued that trained model weights may be considered a derivative work of the training data ([Franceschelli et al., 2024](https://doi.org/10.48550/arXiv.2407.13493)).

## Evidence of value

Tell IDC why the data is worth hosting. Examples:

* the data was collected under a competitive research grant
* the data is described in a peer-reviewed publication
* for manual annotations: the qualifications of the experts who made them
* for ML results: published or otherwise established performance of the model, and documented quality assurance

## De-identification

{% hint style="warning" %}
IDC does not de-identify data, and does not check the results of de-identification. Making sure the data is de-identified is the submitter's responsibility.
{% endhint %}

You have two options:

* **Use an entity IDC already recognizes as qualified to de-identify your type of data.** IDC has already reviewed how such an entity de-identifies data, so it does not review that again for each dataset.
* **De-identify the data yourself, or use an entity IDC has not reviewed before.** In this case you submit documentation describing how the data was de-identified. IDC reviews the documentation, but not the data, and gives you feedback. Acceptance is not guaranteed.

How closely IDC reviews the documentation depends on the type of data:

* **Digital pathology slides in vendor formats** are easier. IDC converts them to DICOM and controls which metadata is carried over.
* **DICOM radiology** is riskier, because identifying information can remain in many places in the DICOM headers and in the pixel data. TCIA is currently the only entity IDC recognizes as qualified to de-identify DICOM radiology.

If you annotate images that are already in IDC, make sure that annotating and converting them does not add identifying information back. See [Avoid adding identifying information](#avoid-adding-identifying-information).

## DICOM format

All data in IDC is in DICOM. If your data is in another format, IDC considers it only when conversion to DICOM is feasible. IDC then either converts it or guides you through the conversion (see [Preparing data in DICOM](preparing-data-in-dicom.md)).

## Annotations of images in IDC

{% hint style="warning" %}
If you annotate or analyze images that are already in IDC, keep track of exactly which images each annotation or result applies to, using their DICOM UIDs. This is a prerequisite for converting your results to DICOM: without it, IDC cannot link your results to the images they describe.
{% endhint %}

The practices below make conversion possible and avoid rework. They are easiest to follow if you plan for them before you start annotating.

### Track the images by UID

* Record the SeriesInstanceUID of every series you annotated or analyzed. For annotations that apply to specific slices, frames or slides, also record the SOPInstanceUID of each image.
* File names, folder paths or patient IDs alone are not enough, because they cannot be matched reliably to the images in IDC.
* If you converted the images to another format for annotation, such as NIfTI, keep the mapping from each converted file back to the series and images it was made from.
* Note which IDC data release you downloaded the images from. In idc-index, `IDCClient.get_idc_version()` returns it.

Every DICOM file carries its UIDs in its header, and the IDC Portal manifests and idc-index list the SeriesInstanceUID of every series.

### Preserve geometry

* Annotate on the original pixel grid of the images.
* If you converted the images to another format for annotation, do not resample, crop, flip or reorient the results. If you had to, record exactly what was done, so that the results can be mapped back to the original images.
* Check how your conversion tool handled each series. Some tools split a series into several volumes, for example by acquisition number or echo, or reorder its slices. Record which images went into each volume. In the RCC-AID dataset, for example, series had been split by acquisition number before annotation, so each label map covered only part of its series (see [rcc-aid-conversion](https://github.com/ImagingDataCommons/rcc-aid-conversion)).
* For point and region annotations, record whether coordinates are in pixels or in physical units. For slides, also record which image and resolution level the coordinates refer to.

### Define every label

* Provide a dictionary that says what each label value or class name means: the structure or finding, and its laterality where that applies. IDC uses it to describe each segment with standard codes (see [Coding schemes](../dicom/coding-schemes.md)).
* For tabular results, such as measurements or radiomics features, define each column and its units, and link each row to the UIDs of the images and to the region it measures.

### Record how the results were made

* whether the annotations are manual, semi-automatic (for example, created by a model and corrected by a person) or automatic
* who made them and their qualifications, per reader when several readers annotated the same images. In the data itself, identify readers by codes (see [Avoid adding identifying information](#avoid-adding-identifying-information)).
* the name, version and settings of the model or software used, including any software used to extract features

DICOM needs this information. A DICOM segmentation, for example, must state whether each segment was made manually, semi-automatically or automatically, and must name the algorithm when one was used.

### Record negative findings

Record which images were reviewed and found to have nothing to annotate, separately from images that were never reviewed. Otherwise the two cannot be told apart: both are images without annotations.

### Avoid adding identifying information

The images in IDC are de-identified. Identifying information about patients (PHI), or about other people (PII), can still be added back while you annotate or convert, especially if you convert to DICOM yourself. Make sure none of the following ends up in your results:

* patient names, medical record numbers, dates or other identifiers in file names, folder paths or labels. Some tools write these into the files they produce.
* names or other details of the people who annotated or converted the data. Software can pick these up from user accounts or computer names. In the data, identify annotators by codes such as "Reader 1" instead, and give their names and qualifications to IDC separately.
* identifying details in free-text fields, such as comments, labels or segment descriptions

If you convert to DICOM yourself, inspect the headers of the files you produce before you submit them, for example with [pydicom](https://pydicom.github.io/) or dcmdump.

Ready to get in touch? See [Contribution process](contribution-process.md).
