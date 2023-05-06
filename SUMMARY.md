# Table of contents

* [Welcome!](README.md)
* [Getting started](getting-started-with-idc.md)
* [Core functions](core-functions-of-idc.md)
* [Frequently asked questions](frequently-asked-questions.md)
* [Support](support.md)
* [Key pointers](idc-key-pointers.md)

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
* [Data versioning](data/data-versioning.md)
* [Organization of data](data/organization-of-data/README.md)
  * [Files and metadata](data/organization-of-data/files-and-metadata.md)
  * [Resolving CRDC Globally Unique Identifiers (GUIDs)](data/organization-of-data/guids-and-uuids.md)
  * [Clinical data](data/organization-of-data/clinical.md)
  * [Organization of data, v2 through V13 (deprecated)](data/organization-of-data/organization-of-data-v2-through-v13-deprecated/README.md)
    * [Files and metadata](data/organization-of-data/organization-of-data-v2-through-v13-deprecated/files-and-metadata.md)
    * [Resolving CRDC Globally Unique Identifiers (GUIDs)](data/organization-of-data/organization-of-data-v2-through-v13-deprecated/guids-and-uuids.md)
    * [Clinical data](data/organization-of-data/organization-of-data-v2-through-v13-deprecated/clinical.md)
  * [Organization of data in v1 (deprecated)](data/organization-of-data/organization-of-data-v1.md)
* [Downloading data](data/downloading-data/README.md)
  * [Downloading data with s5cmd](data/downloading-data/downloading-data-with-s5cmd.md)
* [Data release notes](data/data-release-notes.md)
* [Data known issues](data/data-known-issues.md)

## DICOM

* [Introduction to DICOM](dicom/introduction.md)
* [Data model](dicom/data-model.md)
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
* [Exploring data and cohorts](portal/data-exploration-and-cohorts/README.md)
  * [Exploring imaging data](portal/data-exploration-and-cohorts/exploring-imaging-data.md)
  * [Viewing collections, studies, and series](portal/data-exploration-and-cohorts/viewing-collections-studies-and-series.md)
  * [Understanding cohorts](portal/data-exploration-and-cohorts/understanding-cohorts.md)
* [Cohort manifests](portal/cohort-manifests.md)
* [Visualizing images](portal/visualization.md)
* [Proxy policy](portal/proxy-policy.md)
* [Viewer release notes](portal/viewer-release-notes.md)
* [Portal release notes](portal/release-notes.md)
* [Portal known issues](portal/portal-known-issues.md)

## API

* [Getting started](api/getting-started.md)
* [IDC Data Model Concepts](api/idc-data-model-concepts.md)
* [Accessing the API](api/accessing-the-api.md)
* [Endpoint Details](api/endpoint-details.md)
* [Release notes](api/release-notes.md)

## Cookbook

* [Colab notebooks](cookbook/notebooks.md)
* [BigQuery](cookbook/bigquery.md)
* [Data Studio](cookbook/data-studio/README.md)
  * [Dashboard for your cohort](cookbook/data-studio/cohort-dashboard.md)
  * [More dashboard examples](cookbook/data-studio/more-dashboard-examples.md)
* [Compute engine](cookbook/virtual-machines/README.md)
  * [3D Slicer desktop VM](cookbook/virtual-machines/idc-desktop.md)
  * [Using a BQ Manifest to Load DICOM Files onto a VM](cookbook/virtual-machines/using-a-bq-manifest-to-load-dicom-files-onto-a-vm.md)
  * [Using VS Code with GCP VMs](cookbook/virtual-machines/using-vs-code-with-gcp-vms.md)
* [NCI Cloud Resources](cookbook/nci-data-commons-cloud-resources.md)
