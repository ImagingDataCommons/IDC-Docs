# Getting started

This section describes v1 of the IDC REST API . This API is designed for use by developers of image analysis and data mining tools to directly query the public resources of the IDC and retrieve information into their applications. The API complements the IDC web application but eliminates the need for users to visit the IDC web pages to perform cohort creation, manifest export, and transfer of image data to some local file system.

The IDC API conforms to the [OpenAPI 2.0](https://swagger.io/specification/) specification which "_defines a standard, language-agnostic interface to RESTful APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection_."

{% hint style="info" %}
If you have feedback about the desired features of the IDC API, please let us know via the IDC [support forum](https://discourse.canceridc.dev).
{% endhint %}

The API is a RESTful interface, accessed through web URLs. There is no software that an application developer needs to download in order to use the API. The application developer can build their own access routines using just the API documentation provided. The interface employs a set of predefined query functions that access IDC data sources.

The IDC API is intended to enable exploration of IDC hosted data without the need to understand and use the Structure Query Language \(SQL\). To this end, data exploration capabilities through the IDC API are limited. However, IDC data is hosted using the standard capabilities of the the Google Cloud Platform \(GCP\) Storage \(GCS\) and BigQuery \(BQ\) components. Therefore, all of the capabilities provided by GCP to access GCS storage buckets and BQ tables are available for more advanced interaction with that data using GCP API.

## Other API Documentation

[SwaggerUI](https://swagger.io/tools/swagger-ui/) is a web based interface that allows users to try out APIs and easily view their documentation. You can access the IDC API SwaggerUI [here](https://api-dot-idc-dev.appspot.com/v1/swagger).

This [Google Colab notebook](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/API/notebooks/How_to_use_IDC_APIs.ipynb) serves as an interactive tutorial to accessing the IDC API using Python.

