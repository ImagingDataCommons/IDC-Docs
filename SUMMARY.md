# Table of contents

* [Welcome!](README.md)
* [🚀 Getting started](getting-started-with-idc.md)
* [Core functions](core-functions-of-idc.md)
* [Frequently Asked Questions](frequently-asked-questions.md)
* [Support](support.md)
* [Key pointers](idc-key-pointers.md)
* [Publications](publications.md)
* [IDC team](idc-team.md)
* [Acknowledgments](acknowledgments.md)
* [Jobs](jobs.md)

## Introduction

* [Features](introduction/features.md)
* [Google Cloud Platform (GCP)](introduction/google-cloud-platform/README.md)
  * [Getting started with GCP](introduction/google-cloud-platform/getting-started-with-gcp.md)
* [Amazon AWS platform](introduction/amazon-aws-platform.md)
* [DICOM](introduction/dicom.md)
* [Requesting GCP cloud credits](introduction/requesting-gcp-cloud-credits.md)
* [Requesting AWS cloud credits](introduction/requesting-aws-cloud-credits.md)

## Data

* [Introduction](data/introduction.md)
* [Data model](data/data-model.md)
* [Organization of data](data/organization-of-data/README.md)
  * [Files and metadata](data/organization-of-data/files-and-metadata.md)
  * [BigQuery tables](data/organization-of-data/bigquery-tables.md)
  * [DICOM stores](data/organization-of-data/dicom-stores.md)
  * [Clinical data](data/organization-of-data/clinical.md)
  * [UUIDs and GUIDs](data/organization-of-data/guids-and-uuids.md)
* [Data versioning](data/data-versioning.md)
* [Downloading data](data/downloading-data/README.md)
  * [IDC Portal](data/downloading-data/idc-portal.md)
  * [idc-index and 3D Slicer](data/downloading-data/downloading-data.md)
  * [s5cmd](data/downloading-data/downloading-data-with-s5cmd.md)
  * [DICOMweb](data/downloading-data/dicomweb-access.md)
  * [Directly loading DICOM objects from Google Cloud or AWS in Python](data/downloading-data/direct-loading.md)
  * [Additional tools](data/downloading-data/additional-tools.md)
* [Data release notes](data/data-release-notes.md)
* [Data known issues](data/data-known-issues.md)

## Tutorials

* [Portal tutorial](tutorials/portal-tutorial.md)
* [Python notebook tutorials](https://github.com/ImagingDataCommons/IDC-Tutorials)
* [Slide microscopy](tutorials/slide-microscopy/README.md)
  * [Using QuPath for visualization](tutorials/slide-microscopy/qpath-for-sm-visualization.md)

## DICOM

* [Introduction to DICOM](dicom/introduction.md)
* [DICOM data model](dicom/data-model.md)
* [Original objects](dicom/original-vs-derived-objects.md)
* [Derived objects](dicom/derived-objects/README.md)
  * [DICOM Segmentations](dicom/derived-objects/dicom-segmentations.md)
  * [DICOM Radiotherapy Structure Sets](dicom/derived-objects/dicom-radiotherapy-structure-sets.md)
  * [DICOM Structured Reports](dicom/derived-objects/dicom-structured-reports.md)
* [Coding schemes](dicom/coding-schemes.md)
* [DICOM-TIFF dual personality files](dicom/dicom-tiff-dual-personality-files.md)
* [IDC DICOM white papers](dicom/idc-dicom-white-papers.md)

## Portal

* [Getting started](portal/getting-started.md)
* [Exploring and subsetting data](portal/data-exploration-and-cohorts/README.md)
  * [Configuring your search](portal/data-exploration-and-cohorts/exploring-imaging-data.md)
  * [Exploring search results](portal/data-exploration-and-cohorts/viewing-collections-studies-and-series.md)
  * [Data selection and download](portal/data-exploration-and-cohorts/understanding-cohorts.md)
* [Manifests: selecting data subsets](portal/cohort-manifests.md)
* [Visualizing images](portal/visualization.md)
* [Proxy policy](portal/proxy-policy.md)
* [Portal release notes](portal/release-notes.md)
* [Viewer release notes](portal/viewer-release-notes.md)
* [Portal known issues](portal/portal-known-issues.md)

## API

* [API overview](api/README.md)
* [Getting Started](api/getting-started.md)
* [IDC API Concepts](api/idc-api-concepts.md)
* [Endpoint Details](api/endpoint-details.md)
* [Querying with SQL](api/querying-with-sql.md)
* [Getting the data](api/getting-data.md)

## MCP

* [MCP server](mcp/README.md)
* [Getting started](mcp/getting-started.md)
* [Tools and resources](mcp/tools.md)

## Cookbook

* [Colab notebooks](cookbook/notebooks.md)
* [BigQuery](cookbook/bigquery.md)
* [Data Studio dashboards](cookbook/data-studio/README.md)
  * [Dashboard for your cohort](cookbook/data-studio/cohort-dashboard.md)
  * [More dashboard examples](cookbook/data-studio/more-dashboard-examples.md)
* [ACCESS allocations](cookbook/access-allocations.md)
* [Compute engine](cookbook/virtual-machines/README.md)
  * [3D Slicer desktop VM](cookbook/virtual-machines/idc-desktop.md)
  * [Using VS Code with GCP VMs](cookbook/virtual-machines/using-vs-code-with-gcp-vms.md)
  * [Security considerations](cookbook/virtual-machines/security-considerations.md)
* [NCI Cloud Resources](cookbook/nci-data-commons-cloud-resources.md)

## Archive

* [Archive](archive.md)
