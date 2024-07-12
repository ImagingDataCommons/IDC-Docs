---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🚀 Getting started

We want Imaging Data Commons to be your companion in your cancer imaging research activities - from discovering relevant data to sharing your analysis results and showcasing the tools you developed!&#x20;

<figure><img src=".gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

### **Explore the data available**

Check out [quick instructions](portal/getting-started.md) on how to access and use [IDC Portal](https://portal.imaging.datacommons.cancer.gov/) web application that will help you search, subset and visualize data available in IDC.

IDC Portal is integrated with powerful visualization tools: just with your web browser you will be able to see IDC images and annotations using OHIF Viewer, Slim viewer and VolView!

### **Subset the content you need**

We have many tools to help you search data in IDC, so that you download only what you need!

* you can do basic filtering/subsetting of the data using IDC Portal, but if you are developer, you will want to learn how to use [`idc-index` python package](https://github.com/ImagingDataCommons/idc-index) for programmatic access. [This python notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting\_started/part2\_searching\_basics.ipynb) will introduce you to the basics of `idc-index` for interaction with IDC content.
* search clinical data: many of the IDC collections are accompanied by clinical data, which we parsed for you into searchable tabular representation - no need to download or parse CSV/Excel/PDF files! Dive into searching clinical data using [this notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting\_started/exploring\_clinical\_data.ipynb).
* if advanced content does not scare you, check out [this notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting\_started/part3\_exploring\_cohorts.ipynb) to learn how to search **all** of the metadata accompanying IDC using SQL and Google BigQuery.

### **Download the data you liked**

We provide various tools for downloading data from IDC, as discussed in the [Download documentation page](data/downloading-data/). Access to all data in IDC is free! No registration. No access request forms. No logins.

* once you have `idc-index` python package installed, download from the command line is as easy as running `idc download <manifest_file>`, or `idc download <collection_id>`.&#x20;
* looking for an interactive "point-and-click" application? [3D Slicer IDC Browser extension](https://github.com/ImagingDataCommons/SlicerIDCBrowser) is for you (note that you will only be able to visualize radiology - not microscopy - images in 3D Slicer)

### **Experiment with analysis tools**

We want to make it easier to understand performance of the latest advances in AI on real-world cancer imaging data!

* if you have a Google account, you have free access to Google Colab, which allows you to run python notebooks on cloud VMs equipped with GPU - for free! Combined with `idc-index` for data access, this makes it rather easy to experiment with the latest AI tools! As an example, take a look at [this notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/analysis/MedSAM\_with\_IDC.ipynb) that allows you to apply MedSAM model to IDC data. You will find a growing number of notebooks to help you use IDC in [this repository](https://github.com/ImagingDataCommons/IDC-Tutorials).
* use IDC to develop HuggingFace spaces that demonstrate the power of your models on real data: see [this space](https://huggingface.co/spaces/ImagingDataCommons/SegVolOnIDC) we developed for SegVol
* growing number of AI medical imaging models is being curated on the MHub.ai platform; see [this notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/analysis/mhubai\_tutorial.ipynb) to learn how to apply those models on data from IDC

How about accompanying your next publication by a working demonstration notebook on relevant samples from IDC? You can see an example how we did this in [this recent publication](https://dx.doi.org/10.1016/j.cmpb.2023.107839).

### **Scale the analysis to thousands of cloud VMs**

With the cloud, you can do things that are simply impossible to do with your local resources.&#x20;

* read [this preprint](https://doi.org/10.21203/rs.3.rs-4351526/v1) to learn how we applied TotalSegmentator+pyradiomics to >126,000 of CT scans of the NLST collection using Terra platform, completing the analysis in \~8 hours with the total cost \~$1000
* [this repository](https://github.com/ImagingDataCommons/CloudSegmentator) contains the code we used in the above (this is **really** advanced content!)

### **Share analysis results or annotations**

If you have an algorithm, that you evaluated/published, that can enrich data in IDC with analysis results and you want to contribute those, or if you are a domain expert and would like to publish results of manual annotations you prepared - we want to hear from you!

* IDC maintains a [Zenodo community](https://zenodo.org/communities/nci-idc) where we curate contributions of analysis results and other datasets produced by IDC (see the [expert annotations of the RMS-Mutations-Prediction microscopy images collection](https://zenodo.org/records/10462858) as one example of such contribution)
* through a dedicated Zenodo record you will have a citation and DOI to get credit for your work; your data is ingested from Zenodo into IDC, and citation will be generated for the users of your data in IDC
* once your data is in IDC, it should be easier to discover it, combine with other datasets, visualize and use from analysis workflows (as an example, see [this notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/collections\_demos/rms\_mutation\_prediction/RMS-Mutation-Prediction-Expert-Annotations\_exploration.ipynb) accompanying the RMS annotations)
* email us at [support+submissions@canceridc.dev](https://mail.google.com/mail/?view=cm\&fs=1\&tf=1\&to=support+submissions@canceridc.dev) to inquire about contributing your annotations/analysis results to IDC!

### Questions?

Join [IDC forum](https://discourse.canceridc.dev) with any inquiries about IDC - we want to hear from you! As you will see from the historical posts, we typically respond to user questions very quickly.&#x20;
