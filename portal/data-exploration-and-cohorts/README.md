# Exploring and subsetting data

<div align="center"><figure><img src="https://github.com/ImagingDataCommons/IDC-Docs/releases/download/v20/amazed_window.png" alt="" width="375"><figcaption></figcaption></figure></div>

## Overview

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

Components on the left side of the page give you controls for configuring your selection:

* **Search scope** allows you to limit your search to just the specific programs, collections and analysis results (as discussed in the documentation of the [IDC Data model](../../data/data-model.md)).&#x20;
* **Search configuration** gives you access to a small set of metadata attributes to select DICOM studies (where "DICOM studies" fit into IDC data model is also discussed in the [IDC Data model](../../data/data-model.md) page) that contain data that meets the search criteria.&#x20;

Panels on the right side will automatically update based on what you select on the left side!

* **Selection configuration** reflects the active search scope/filters in the **Cohort Filters** section. You can download all of the studies that match your filters. Below you will see the **Cart** section. Cart is helpful when selecting data by individual filters is too imprecise, and you want to have more granular control over your selection by selecting specific collections/patients/studies/series.
* **Filtering results** section consists of the tables containing matching content that you can navigate following IDC Data model: first table shows the matching collections, selecting a collection will list matching cases (patients), selection of a case will populate the next table listing matching studies for the patient, and finally selecting a study will expand the final table with the list of series included in the study.

In the following sections of the documentation you will learn more about each of the items we just discussed.
