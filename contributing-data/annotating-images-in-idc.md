# Annotating images in IDC

This page is for anyone who annotates or analyzes images that are already in IDC and plans to contribute the results: manual annotations, and results such as segmentations made by ML models or radiomics features. The practices below make conversion to DICOM possible and avoid rework. They are easiest to follow if you plan for them before you start annotating.

Tracking the images by their DICOM UIDs is a [requirement](requirements.md#annotations-of-images-in-idc): without the UIDs, IDC cannot link your results to the images they describe.

## Track the images by UID

* Record the SeriesInstanceUID of every series you annotated or analyzed. For annotations that apply to specific slices, frames or slides, also record the SOPInstanceUID of each image.
* File names, folder paths or patient IDs alone are not enough, because they cannot be matched reliably to the images in IDC.
* If you converted the images to another format for annotation, such as NIfTI, keep the mapping from each converted file back to the series and images it was made from.
* Note which IDC data release you downloaded the images from. In idc-index, `IDCClient.get_idc_version()` returns it.

Every DICOM file carries its UIDs in its header, and the IDC Portal manifests and idc-index list the SeriesInstanceUID of every series.

## Preserve geometry

* Annotate on the original pixel grid of the images.
* If you converted the images to another format for annotation, do not resample, crop, flip or reorient the results. If you had to, record exactly what was done, so that the results can be mapped back to the original images.
* Check how your conversion tool handled each series. Some tools split a series into several volumes, for example by acquisition number or echo, or reorder its slices. Record which images went into each volume. In the RCC-AID dataset, for example, series had been split by acquisition number before annotation, so each label map covered only part of its series (see [rcc-aid-conversion](https://github.com/ImagingDataCommons/rcc-aid-conversion)).
* For point and region annotations, record whether coordinates are in pixels or in physical units. For slides, also record which image and resolution level the coordinates refer to.

## Define every label

* Provide a dictionary that says what each label value or class name means: the structure or finding, and its laterality where that applies. IDC uses it to describe each segment with standard codes (see [Coding schemes](../dicom/coding-schemes.md)).
* For tabular results, such as measurements or radiomics features, define each column and its units, and link each row to the UIDs of the images and to the region it measures.

## Record how the results were made

* whether the annotations are manual, semi-automatic (for example, created by a model and corrected by a person) or automatic
* who made them and their qualifications, per reader when several readers annotated the same images. In the data itself, identify readers by codes (see [Avoid adding identifying information](#avoid-adding-identifying-information)).
* the name, version and settings of the model or software used, including any software used to extract features

DICOM needs this information. A DICOM segmentation, for example, must state whether each segment was made manually, semi-automatically or automatically, and must name the algorithm when one was used.

## Record negative findings

Record which images were reviewed and found to have nothing to annotate, separately from images that were never reviewed. Otherwise the two cannot be told apart: both are images without annotations.

## Avoid adding identifying information

The images in IDC are de-identified. Identifying information about patients (PHI), or about other people (PII), can still be added back while you annotate or convert, especially if you convert to DICOM yourself. Make sure none of the following ends up in your results:

* patient names, medical record numbers, dates or other identifiers in file names, folder paths or labels. Some tools write these into the files they produce.
* names or other details of the people who annotated or converted the data. Software can pick these up from user accounts or computer names. In the data, identify annotators by codes such as "Reader 1" instead, and give their names and qualifications to IDC separately.
* identifying details in free-text fields, such as comments, labels or segment descriptions

If you convert to DICOM yourself, inspect the headers of the files you produce before you submit them, for example with [pydicom](https://pydicom.github.io/) or dcmdump.

Next: [Preparing data in DICOM](preparing-data-in-dicom.md) covers the conversion itself. Get in touch with IDC before you spend much time on it (see [Contribution process](contribution-process.md)).
