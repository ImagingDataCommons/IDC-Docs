# Accessing the API

## **IDC API UI**

The [IDC API UI](https://api.imaging.datacommons.cancer.gov/v2/swagger#/) can be used to see details about the syntax for each call, and also provides an interface to test requests.

### Make a Request

For a quick demonstration of the syntax of an API call, test the [GET/collections](https://api.imaging.datacommons.cancer.gov/v2/swagger#/data%20model/getCollections) request. You can experiment with this endpoint by clicking the ‘Try it out’ button.

The API will return collection metadata for the current IDC data version. The request can be run by clicking on the  ‘Execute’ button.

### **Request Response**

The Swagger UI submits the request and shows the _curl_ command that was submitted. The ‘Response body’ section will display the response to the request. The expected format of the response to this API request is shown below:

<pre><code><strong>{
</strong>  "collections": [
    {
      "cancer_type": "string",
      "collection_id": "string",
      "date_updated": "string",
      "description": "string",
      "doi": "string",
      "image_types": "string",
      "location": "string",
      "species": "string",
      "subject_count": 0,
      "supporting_data": "string",
    }
  ],
  "code": 200
}
</code></pre>

The actual JSON formatted response can be downloaded by selecting the ‘Download’ button.

### Authenticating to the UI

Some of the API calls require authentication. This is denoted by a small lock symbol. Authentication can be performed by clicking on the ‘Authorize’ button at the top right of the page.

The syntax for all of API data structures is detailed at the bottom of the UI page.API Endpoints

## **Command line API access**

The API can be accessed from the command line using curl or wget. Here we discuss using curl for this purpose.

### Make a request

You access an API endpoint by sending an HTTP request to the IDC API server. The server replies with a response that either contains the data you requested, or a status indicator. An API request URL has the following structure:&#x20;

`<BaseURL><API version><QueryEndpoint>?<QueryParameters>.` \


The \<BaseURL> of the IDC API is `https://api.imaging.datacommons.cancer.gov.`\
For example, this _curl_ command requests metadata on all IDC collections from the V2 API:

`curl -X GET "https://api.imaging.datacommons.cancer.gov/v2/collections" -H "accept: application/json"`\
Note, also, that the HTTP method defaults to GET. However, a POST or DELETE HTTP method must be specified with the -X parameter.

The IDC API UI displays the curl commands which it issues and thus can be a good reference when constructing your own curl commands.

### **Authorization**

Some of the API endpoints, such as _/collections_ and _/cohorts/preview_, can be accessed without authorization. APIs that access user specific data, such as saved cohorts, necessarily require account authorization.

To access those APIs that require IDC authorization, you will need to generate a credentials file. To obtain your credentials:

* Clone the [idc\_auth.py](https://github.com/ImagingDataCommons/IDC-API/blob/master/scripts/idc\_auth.py) script to your local machine.
* Execute the `idc_auth.py` script, e.g.:\
  `$ python ./idc_auth.py`\
  Refer to the `idc_auth.py` file for detailed instructions.

The[ jq JSON command line processor](https://jqlang.github.io/jq/manual/) is useful when dealing with JSON in the command line context. Assuming jq is installed, and that idc\_auth.py has created the credentials file `~/.idc_credentials` (the default location), then the following will extract the id token to a variable:

`$ TOKEN=$(more ~/.idc_credentials| jq -r '.["token_response"]["id_token"]')`

and can be used to authenticate to the API to get a list of your cohorts:

`$ curl -X GET "https://api.imaging.datacommons.cancer.gov/v2/cohorts" -H "accept: application/json" -H "Authorization: Bearer $TOKEN"`

If you pipe the result to jq:\
`$ curl -X GET "https://api.imaging.datacommons.cancer.gov/v2/cohorts" -H "accept: application/json" -H "Authorization: Bearer $TOKEN" | jq`

Then you should see something like this:

{% code fullWidth="false" %}
```
{
  "code": 200,
  "cohorts": [
        {
      "cohort_id": 1103,
      "description": "Test description",
      "filterSet": {
        "filters": {
          "Modality": [
            "CT",
            "MR"
          ],
          "collection_id": [
            "tcga_read"
          ],
          "race": [
            "WHITE"
          ]
        },
        "idc_data_version": "14.0"
      },
      "name": "testcohort",
      "owner": "John Doe",
      "permission": "OWNER"
    },
    {
      "cohort_id": 1172,
      "description": null,
      "filterSet": {
        "filters": {
          "CancerType": [
            "Ovarian Cancer"
          ]
        },
        "idc_data_version": "16.0"
      },
      "name": "ovarian",
      "owner": "John Doe",
      "permission": "OWNER"
    }
  ]
}
```
{% endcode %}

## Programmed Access

We expect that most API access will be programmed access, and, moreover, that most programmed access will be within a Python script using the [Python Requests package](https://pypi.org/project/requests/). This usage is covered in detail (along with details on each of the IDC API endpoints) in the [How\_to\_use\_the\_IDC\_V2\_API](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/API/notebooks/How\_to\_use\_IDC\_APIs.ipynb) Google Colab notebook. Here we provide just a brief overview.

In Python, we can issue the following request to obtain a list of the collections in the current IDC version:

<pre><code><strong>response = requests.get("https://api.imaging.datacommons.cancer.gov/v2/collections")
</strong><strong>collections = response.json['collections']
</strong></code></pre>

## Paged queries

The _/cohorts/manifest/preview and /cohorts/manifest/{cohort\_id}_ endpoints are paged. That is, several calls of the API may be required to return all the data resulting from such a query. Each endpoint accepts a _**page\_size**_ parameter in the _manifestBody_ or _manifestPreviewBody_ that is the maximum number of rows that the client wants the server to return. The returned data from each of these APIs includes a _next\_page_ value. _next\_page_ is null if there is no more data to be returned. If _next\_page_ is non-null, then more data is available.

In the case that the returned _next\_page_ value is not null, the _/cohorts/manifest/nextPage or /cohorts/manifest/preview/nextPage_ endpoint can be accessed, passing the _next\_page_ token returned by the previous call.

## Timeouts

The manifest endpoints may return an HTTP 202 error. This indicates that the request was accepted but processing timed out before it was completed. In this case, the client should resubmit the request including the next\_page token that was returned with the error response.
