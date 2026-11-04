# Preparing data in DICOM

{% hint style="warning" %}
Converting data to DICOM is currently not easy. Please get in touch with IDC before you spend much time on conversion. IDC is happy for you to do the conversion yourself and will advise and guide you. Conversion results that have issues may need to be redone, and we do not want to waste your time.
{% endhint %}

To convert annotations of images in IDC, you need to know exactly which images, identified by their DICOM UIDs, each annotation applies to. See [Annotating images in IDC](annotating-images-in-idc.md).

## Choosing a DICOM object

IDC will confirm the right DICOM representation for your data before you start. As a rough guide:

| Data | DICOM object | Read more |
| --- | --- | --- |
| Segmentations of radiology or pathology images | Segmentation (SEG) | [DICOM Segmentations](../dicom/derived-objects/dicom-segmentations.md) |
| Measurements, qualitative evaluations, point and region annotations | Structured Report (SR) | [DICOM Structured Reports](../dicom/derived-objects/dicom-structured-reports.md) |
| Large numbers of annotations on slides, such as nuclei outlines | Microscopy Bulk Simple Annotations (ANN) | [highdicom user guide](https://highdicom.readthedocs.io/en/latest/ann.html) |
| Whole slide images | Slide Microscopy (SM) | [Implementing the DICOM standard for digital pathology](https://doi.org/10.4103/jpi.jpi_42_18) |

Annotations need standard codes to describe what was annotated. See [Coding schemes](../dicom/coding-schemes.md).

## Tools

Open-source tools that can help:

* [dcmqi](https://github.com/QIICR/dcmqi): command-line tools that need no programming; you describe the content in JSON files. They convert segmentations from research formats such as NIfTI and NRRD into DICOM SEG, and tabular data, such as radiomics features derived from segmentations, into DICOM SR. The tools can be installed with `pip install dcmqi` (see the [PyPI dcmqi package](https://pypi.org/project/dcmqi/)).
* [highdicom](https://github.com/ImagingDataCommons/highdicom): a Python library for creating DICOM SEG, SR, ANN and other objects. Using it means writing Python code.
* [wsidicomizer](https://github.com/imi-bigpicture/wsidicomizer): converts whole slide images from vendor formats into DICOM SM
* [dciodvfy](https://www.dclunie.com/dicom3tools.html), part of dicom3tools: checks DICOM files against the standard (can be installed from the [PyPI dicom3tools package](https://pypi.org/project/dicom3tools/))

## A complete example

[rcc-aid-conversion](https://github.com/ImagingDataCommons/rcc-aid-conversion) converts the RCC-AID kidney annotations from NIfTI to DICOM SEG, with notes on terminology and on the problems found along the way.
