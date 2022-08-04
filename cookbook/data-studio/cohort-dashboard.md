---
description: >-
  Use IDC-provided DataStudio template to build a custom dashboard for your
  cohort
---

# Dashboard for your cohort

You can use [this DataStudio template](http://bit.ly/3jdCmON) to build a custom dashboard for your own cohort, which will look like the screenshot below in three relatively simple steps.

![Screenshot of the DataStudio dashboard template you can use to explore your cohort.](<../../.gitbook/assets/image (11).png>)

**Step 1: Prepare the manifest BigQuery table**

Export the cohort manifest as a BigQuery table, and take note of the location of the resulting table.

![Name of the BQ table you will need is highlighted with the red rectangle.](<../../.gitbook/assets/image (14).png>)

**Step 2: Duplicate the template**

Open the dashboard template following this link: [http://bit.ly/3jdCmON](http://bit.ly/3jdCmON), and click "Use template" to make a copy of the dashboard.

![](<../../.gitbook/assets/image (16).png>)

When prompted, do not change the default options, and click "Copy Report".

![](<../../.gitbook/assets/image (13).png>)

**Step 3: Configure data source**

Select "Resource > Manage added data sources"

![](<../../.gitbook/assets/image (5).png>)

Select "Edit" action:

![](<../../.gitbook/assets/image (4).png>)

Update the custom query as instructed. This will select all of the DICOM metadata available for the instances in your cohort.

![](<../../.gitbook/assets/image (12).png>)

For example, if the location of your manifest table is `canceridc-user-data.user_manifests.manifest_cohort_101_20210127_213746`, the custom query that will join your manifest with the DICOM metadata will be the following:

```sql
SELECT
  all_of_idc.*
FROM
  `canceridc-user-data.user_manifests.manifest_cohort_101_20210127_213746` AS my_cohort
JOIN
  `canceridc-data.idc_current.dicom_all` AS all_of_idc
ON
  all_of_idc.SOPInstanceUID = my_cohort.SOPInstanceUID
```

Once you updated the query, click "Reconnect" in the upper right corner.&#x20;

{% hint style="warning" %}
Make sure you select a valid Billing Project that you can use to support the queries!
{% endhint %}

![](<../../.gitbook/assets/image (22).png>)

Accept the below, if prompted (you may also be notified about changes to the schema of the table, so the message may be different).

![](<../../.gitbook/assets/image (8).png>)

Click "Done" on the next screen:

![](<../../.gitbook/assets/image (6).png>)

Click "Close" on the next screen:

![](<../../.gitbook/assets/image (10).png>)

**You are Done!** The dashboard for your cohort is now live: you can "View" it to interact with the content, you can edit it to explore additional attributes in the cohort, and you can choose to keep it private or share with a link!

![](<../../.gitbook/assets/image (2).png>)
