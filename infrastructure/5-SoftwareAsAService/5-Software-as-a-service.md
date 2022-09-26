## 5 Software as a Service

The Google Cloud Platform (GCP) provides a wide range of “Software as a Service” (SaaS) offerings that we use to implement IDC. We also use third-party providers such as GitHub and CircleCI to manage code versioning and deployment. In this section, we describe how to configure these services to support our IDC Web App, Cron, and API components: 
* **G Suite:** Used to provide an organizational framework for owning IDC Google cloud projects, and for supporting enforceable password requirements on Google IDs.
* **Google Cloud** Storage: Used to store files, including files needed to configure and deploy the Web Application.
* **CircleCI:** Used to build and deploy the Web App, Cron, the API, the Throttle Proxy, and the OHIF and SliM viewers to GCP.  
* **Google IAM:** Used to assign roles to staff, and to create service accounts and assign roles to them.
* **CloudSQL:** Used by the Web App for running the Django web application, storing user-defined (i.e. cohorts) data, and modeling the data available in IDC’s data sources.
* **BigQuery:** Used to hold IDC data and metadata.
* **Google Cloud Logging (formerly Stackdriver):** Used to monitor and store logs for IDC components.
* **Google Cloud Monitoring (formerly Stackdriver Alerts):** Used for notifications when logged events diverge from expected values.
* **Compute Engine:** Used to run the Apache Solr indexed searcher, and to process bucket usage log files into BQ tables
* **Project Firewall Rules:** Used to constrain communications between components in the project.
* **AppEngine:** We use both App Engine Flex and Standard to run the Web App (Flex), Cron services (Standard), the API (Standard), and the Throttle Proxy (Standard).
* **Google Load Balancer:** Required to pin the web portal to a static IP address that can then be resolved using NCI DNS servers.
* **Google Cloud Functions:** Used to process budget notifications.
