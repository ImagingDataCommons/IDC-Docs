# Amazon AWS platform

[Amazon Web Services (AWS)](https://aws.amazon.com/) provide cloud-based scalable solutions to support computation, storage and a range of other services. Starting from release v14, IDC files are replicated in AWS storage buckets (see [IDC dataset entry](https://registry.opendata.aws/nci-imaging-data-commons/) in the AWS Open Data Registry), and their AWS locations are indexed in IDC metadata tables. If you are using AWS to meet your analysis needs, you can download IDC data from AWS.

Note that no matter whether you are [downloading IDC data](../data/downloading-data/) from GCP or AWS, you do not need to log in or pay for data egress!

As of IDC data release v14, in order to build a manifest for downloading IDC files from AWS, you will have to either use the IDC Portal (this can be done without a Google identity, and does not require a GCP project), or by querying GCP BigQuery tables (you will need to complete [GCP prerequisites](google-cloud-platform/getting-started-with-gcp.md) for this step).
