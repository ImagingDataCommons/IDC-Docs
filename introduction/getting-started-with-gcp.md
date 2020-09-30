# Getting started with GCP

Google Cloud platform provides a range of solutions to better understand and analyze data hosted by IDC. Depending on what you want to do \(see the range of options [here](../getting-started-with-idc.md)\), you may need to complete one or more of the following steps below.

**Obtain a Google identity**

* Do you or your institution already have a Google identity, such as a Gmail account? If so, you can proceed to the next step.
* If not, it only takes a minute to [create a Google identity](https://accounts.google.com/signup/v2/webcreateaccount?dsh=308321458437252901&continue=https%3A%2F%2Faccounts.google.com%2FManageAccount&flowName=GlifWebSignIn&flowEntry=SignUp#FirstName=&LastName=). Note that you do NOT need a Gmail email account - [you can use your non-Gmail email address to create one instead](https://support.google.com/accounts/answer/27441?hl=en#existingemail).

**Set up a Google Cloud Project**

You can also follow the tutorial 

* See Google’s documentation about how to [create a Google Cloud Project](https://cloud.google.com/resource-manager/docs/creating-managing-projects).
* Learn about how to [add members and roles to a project](https://cloud.google.com/iam/docs/quickstart).
* [Enable Required Google Cloud APIs](https://cloud.google.com/apis/docs/getting-started#enabling_apis).

**Set up billing for your project**

{% hint style="danger" %}
We can't stress enough how important it is to be diligent in tracking your usage of GCP resources!

* **Be sure to shut down anything you aren't using** - free trial credits, IDC-provided credits or your credit card will be charged otherwise for the resources you are not using.
* Be careful with your login information. If someone takes over your account they **could run up a huge bill that you will be responsible for paying**.
* Unless you are not concerned about billing, remember to **SHUT DOWN THE MACHINE** when you aren't using it! You are billed continuously while the VM instance is running.
* Even after you stop the VMs, you keep paying for the disk storage attached to those machines! You can delete the VM instances to stop incurring those costs.
{% endhint %}

{% hint style="info" %}
You can use [Budget Alerts](https://cloud.google.com/billing/docs/how-to/budgets) to monitor your adherence to a budget!
{% endhint %}

Depending on your situation, you can consider the following mechanisms to support your work on the cloud:

* Take advantage of a one-time [$300 Google Credit](https://cloud.google.com/free/) \(note that you will need to provide credit card information for verifying you are a real person - you won’t be charged unless you manually upgrade to a paid account\)
* If you have already used this one-time offer \(or there is some other reason you cannot use it\), you can [apply for "early adopter" Cloud Credits from IDC](requesting-gcp-cloud-credits.md).
* You can use a credit card to cover the cloud charges.

