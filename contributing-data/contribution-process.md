# Contribution process

This page describes how a contribution made directly to IDC proceeds, from the first email to publication. Before you start, check the [Requirements](requirements.md).

## Steps

1. **Get in touch first.** Email [support+submissions@canceridc.dev](mailto:support+submissions@canceridc.dev) before you convert or upload anything. Keep this first email brief. A sentence or two on each of the points below is enough for IDC's initial assessment, and you don't need to prepare detailed documentation at this stage. Cover:
   * what the data is: images or derived data, modalities, cancer types, number of subjects, approximate size
   * for derived data, which IDC collections it applies to, and whether you recorded the DICOM UIDs of the images you annotated (see [Annotating images in IDC](annotating-images-in-idc.md#track-the-images-by-uid))
   * the current format, and where the data is now (for example, whether it is already published with a DOI)
   * how it was de-identified, and by whom
   * the license you intend to use
   * evidence of value, as described in [Requirements](requirements.md#evidence-of-value)
   * whether clinical data accompanies the images
2. **Initial assessment.** IDC reviews your email and replies with feedback. If the proposal looks promising, IDC sends you a metadata template to describe the dataset.
3. **Detailed description.** You fill in the metadata template, describing the dataset in detail. Unless the data was de-identified by an entity IDC already recognizes, you also submit documentation of how it was de-identified (see [De-identification](requirements.md#de-identification)). IDC reviews these documents, may ask for more information, and decides whether to accept the contribution.
4. **Upload.** If your contribution is accepted, IDC gives you a location to upload your data and explains how.
5. **Conversion.** If your data is not in DICOM, IDC converts it, or you convert it with IDC's guidance (see [Preparing data in DICOM](preparing-data-in-dicom.md)). IDC works with you to choose the appropriate DICOM object type.
6. **Review before release.** IDC gives you access to view the converted data and its metadata, so you can confirm it is correct before it is published.
7. **DOI.**
   * If your data was already in DICOM and published with a DOI, that DOI is reused.
   * If IDC converted your data, IDC creates a record with its own DOI for the DICOM version in the [NCI Imaging Data Commons Zenodo community](https://zenodo.org/communities/nci-idc/). The record always cites and acknowledges your original dataset. When that dataset has a DOI, the record's metadata links to it as the source the DICOM version is derived from (Zenodo's "is derived from" relation). Its co-authors are the people who collected the data, the people outside the IDC team who helped with the conversion, and the IDC team members who worked on the conversion.
8. **Release.** Your data is published in a future IDC data release and listed in the [data release notes](../data/data-release-notes.md). IDC does not commit to a date: timing depends on the data and on IDC's capacity.

## Clinical data

Clinical data that accompanies the images is welcome. It must be:

* de-identified under the same rules as the images
* supplied as tables (CSV or XLSX) with a data dictionary that defines each column and value
* linkable to the images by patient, using the DICOM PatientID

IDC loads the dictionaries and the clinical data tables into BigQuery tables, and also makes them available and searchable through the [idc-index](https://pypi.org/project/idc-index/) Python package. See [Clinical data](../data/organization-of-data/clinical.md).

## After publication

**Corrections and additions.** Email [support+submissions@canceridc.dev](mailto:support+submissions@canceridc.dev) to request changes. Changes are published in a later IDC release as a new version of your data, and the earlier versions stay available so that analyses done on them can be reproduced. See [Data versioning](../data/data-versioning.md).

**Withdrawal.** If identifying information about patients (PHI/PII) is found in published data, IDC removes the affected data from all versions. Any other request to withdraw published data is reviewed case by case by IDC stakeholders.
