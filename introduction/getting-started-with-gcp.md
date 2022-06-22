# Getting started with GCP

{% hint style="info" %}
Whether you are new to the cloud, or you consider yourself an expert, we encourage you to apply for a free Google cloud credits that we provide to our users to support cancer imaging research projects that work with Imaging Data Commons. All reasonable requests will receive a $300 allocation of credits that do not expire, and we will not require you to provide a credit card information to verify your identity. All you have to do is fill out and submit [this application form](https://docs.google.com/forms/d/e/1FAIpQLSfXvXqficGaVEalJI3ym6rKqarmW\_YUUWG6A4U8pclvR8MmRQ/viewform).
{% endhint %}

You are also encouraged to review the slides in the following presentation that provides an introduction into GCP, and shares some best practices the its usage.

> W. Longabaugh. _Introduction to Google Cloud Platform_. Presented at MICCAI 2021. ([slides in Google Slides](https://docs.google.com/presentation/d/1HNZ34xkZCXt6WRDcEtmAUGNq5TM0xzPaK7sojKJfoBc/edit?usp=sharing))

Google Cloud platform provides a range of solutions to better understand and analyze data hosted by IDC. Depending on what you want to do (see the range of options [here](../getting-started-with-idc.md)), you may need to complete one or more of the following steps below.

The steps concerning creating a Google Cloud project and setting up billing are covered in [this short video tutorial](https://youtu.be/i08S0KJLnyw).

### **Obtain a Google identity**

Do you have a Google identity? If so, you can proceed to the next step.

If not, it only takes a minute to [create a Google identity](https://accounts.google.com/signup/v2/webcreateaccount?dsh=308321458437252901\&continue=https%3A%2F%2Faccounts.google.com%2FManageAccount\&flowName=GlifWebSignIn\&flowEntry=SignUp#FirstName=\&LastName=). Note that you do NOT need a Gmail email account - [you can use your non-Gmail email address to create one instead](https://support.google.com/accounts/answer/27441?hl=en#existingemail).

### **Set up a Google Cloud Project**

To perform queries against IDC BigQuery tables you will need a cloud project. You can get started with Google Cloud free project with the following steps (they are also illustrated in [this short video](https://youtu.be/i08S0KJLnyw)):

1. Go to [https://console.cloud.google.com/](https://console.cloud.google.com/), and accept Terms and conditions.
2. Click "Select a project" button in the upper left corner of the screen, and then click "New project".

Additional reading materials:

* See Google’s documentation about how to [create a Google Cloud Project](https://cloud.google.com/resource-manager/docs/creating-managing-projects).
* Learn about how to [add members and roles to a project](https://cloud.google.com/iam/docs/quickstart).
* [Enable Required Google Cloud APIs](https://cloud.google.com/apis/docs/getting-started#enabling\_apis).

### Locate IDC metadata tables in Cloud BigQuery console

IDC is using BigQuery for managing metadata for the hosted data. In order to locate the tables that contain such metadata, complete the following steps:

1. Open BigQuery console: [https://console.cloud.google.com/bigquery](https://console.cloud.google.com/bigquery?project=idc-tcia)
2. Click "+ ADD DATA" button, and select "Pin a project > Enter project name"
3. Type `bigquery-public-data` in the text box and click "PIN" button
4. In the left panel, expand the `bigquery-public-data` drop-down, and navigate to the items called `idc_v1`, `idc_v2`, ..., `idc_current`, which are the datasets containing metadata tables maintained by IDC. Numbered datasets correspond to the IDC data versions documented in [Data Release Notes](../data/data-release-notes.md). `idc_current` is an alias that always points to the latest IDC version.

### **OPTIONAL: Set up billing for your project**

You will not need to set up billing for your project to do basic operations with IDC, such as running Colab notebooks, or executing queries, as long as you stay within the [GCP free tier](https://cloud.google.com/free).&#x20;

You will need to set up project billing if you want to launch your own VMs, or use resources beyond the free usage tier.

If you are just starting, it may be easiest to take advantage of the IDC "early adopter" free cloud credits allocation by filling out [this form](requesting-gcp-cloud-credits.md).

{% hint style="danger" %}
Once you set up billing, we can't stress enough how important it is to be diligent in tracking your usage of GCP resources!

* **Be sure to shut down anything you aren't using** - free trial credits, IDC-provided credits or your credit card will be charged otherwise for the resources you are not using.
* Be careful with your login information. If someone takes over your account they **could run up a huge bill that you will be responsible for paying**.
* Unless you are not concerned about billing, remember to **SHUT DOWN THE MACHINE** when you aren't using it! You are billed continuously while the VM instance is running.
* Even after you stop the VMs, you keep paying for the disk storage attached to those machines! You can delete the VM instances to stop incurring those costs.
{% endhint %}

{% hint style="info" %}
You can use [Budget Alerts](https://cloud.google.com/billing/docs/how-to/budgets) to monitor your adherence to a budget!
{% endhint %}
