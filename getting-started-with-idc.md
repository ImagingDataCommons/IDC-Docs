# Getting started with IDC

While the possibilities of what you can do with the IDC hosted data are endless, we 

1. use the IDC portal to build a cohort of cases that are needed for your use case: see [demo video - Introduction to IDC portal](https://youtu.be/GE1rimW0EtM)
2. interrogate the DICOM metadata directly from a Colab notebook \(or BigQuery Console\): see [demo notebook - Exploration of LIDC collection](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/notebooks/LIDC_exploration.ipynb)
3. use Google Collab notebooks to operate on the cohort: see [demo notebook - Working with IDC cohorts](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/notebooks/Cohort_download.ipynb) and [the accompanying tutorial video](https://youtu.be/LeXBmnjYq1Q)
4. use Google DataStudio to build interactive data dashboards for your cohort: see [example of using DataStudio to build a custom dashboard of IDC content](https://datastudio.google.com/reporting/20683e72-32a9-4c87-8c0f-bfdf62454b75)
5. deploy existing tools on a GCP Virtual Machine to support visualization and analysis of the data from your cohort: demo and details are coming!

| What you can do | What you need to do it |
| :--- | :--- |
| Interact with the IDC portal to explore and build a cohort | no prerequisites |
| Visualize images hosted by IDC | no prerequisites, daily quota in place based on your IP |
| Save cohorts using IDC portal | account on IDC portal |
| Save your IDC cohort under your project in Google Cloud | Google account, Google Cloud project, free operations under [BigQuery sandbox](https://cloud.google.com/bigquery/docs/sandbox) subject to [limits](https://cloud.google.com/bigquery/docs/sandbox#limits) |
| Use [Google BigQuery](https://cloud.google.com/bigquery) to further explore metadata for your cohort | Google account, Google Cloud project, free operations under [BigQuery sandbox](https://cloud.google.com/bigquery/docs/sandbox) subject to [limits](https://cloud.google.com/bigquery/docs/sandbox#limits) |
| Use [Google Colab](https://colab.research.google.com/) notebooks to further explore and do preliminary analysis of your cohort | Google account, Google Cloud project \(billing will need to be set up if you want to download data from IDC storage buckets to the Colab instance!\), free operations under [Google Cloud free tier](https://cloud.google.com/free/docs/gcp-free-tier) |
| Use [Google DataStudio](https://datastudio.google.com/) to build dashboards for your cohort | Google account |
| Use Google Compute or AI Notebooks Virtual Machines to do the analysis of the cohort | Google account, Google Cloud project, billing set up \(i.e., credit card or cloud credits needed\) |

