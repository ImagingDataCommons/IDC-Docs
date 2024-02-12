# Accessing the API

The following characteristics apply to all IDC APIs:

* You access a resource by sending an HTTP request to the IDC API server. The server replies with a response that either contains the data you requested, or a status indicator.
*   An API request URL has the following structure: \<BaseURL>\<API version>\<QueryEndpoint>?\<QueryParameters>. For example, this _curl_ command is a request for metadata on all IDC collections:

    `curl -X GET "https://api.imaging.datacommons.cancer.gov/v1/collections" -H "accept: application/json"`

## API Endpoints

**Authorization**

Some of the APIs, such as _/collections_ and _/cohorts/preview_, can be accessed without authorization. APIs that access user specific data, such as cohorts, necessarily require account authorization.

To access these APIs that require IDC authorization, you will need to generate a credentials file. To obtain your credentials:

* Clone the [IDC-Examples git repository](https://github.com/ImagingDataCommons/IDC-Examples) to your local machine.
* Execute the `idc_auth.py` script either through the command line or from within python. Refer to the `idc_auth.py` file for detailed instructions.

Example usage of the generated authorization is demonstrated by code in the [How\_to\_use\_IDC\_APIs.ipynb](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/API/notebooks/How\_to\_use\_IDC\_APIs.ipynb) Google Colab notebook.

### Paged queries

Several IDC APIs, specifically _/cohorts/manifest/preview, /cohorts/manifest/{cohort\_id}, /cohorts/query/preview, /cohorts/query/{cohort\_id}, and /dicomMetadata, are paged. That is, several calls of the API may be required to return all the data resulting from such a query. Each accepts a \_page\_size_ query parameter that is the maximum number of objects that the client wants the server to return. The returned data from each of these APIs includes a _next\_page_ value. _next\_page_ is null if there is no more data to be returned. If _next\_page_ is non-null, then more data is available.

There are corresponding queries, /cohorts/manifest/nextPage, /cohorts/query/nextPage, and _/dicomMetadata/nextpage_ endpoints, that each accept two query parameters: _next\_page_, and _page\_size._ In the case that the returned _next\_page_ value is not null, the corresponding ../nextPage endpoint is accessed, passing the _next\_page_ token returned by the previous call.

### Timeouts

The manifest and query endpoints may return an HTTP 202 error. This indicates that the request was accepted but processing timed out before it was completed. In this case the client should resubmit the request including the next\_page token that was returned with the error response.
