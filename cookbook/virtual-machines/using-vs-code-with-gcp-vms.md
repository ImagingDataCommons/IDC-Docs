# Using VS Code with GCP VMs

[Visual Studio Code](https://code.visualstudio.com/) has a useful feature of allowing you to develop code on a remote VM from the convenience of your desktop. You can follow the steps below to configure your development environment for this task.

## Prerequisites

* \`\`[`gcloud` SDK](https://cloud.google.com/sdk/docs/install) installed on your computer
* [Visual Studio Code](https://code.visualstudio.com/) installed on your computer
* A GCP VM you want to use for code development is up and running

## **Step 1: Install "Remote - SSH" extension**

![](../../.gitbook/assets/image%20%2819%29.png)

## **Step 2: Populate SSH config files**

Run the following command to populate SSH config files with host entries for each VM instance you have running

```bash
$ gcloud compute config-ssh
```

## Step 3: Connect to host

If the previous step completed successfully, you should see the running VMs in the Remote Explorer of VS Code, as in the screenshot below, and should be able to open a new session to those remove VMs.

![](../../.gitbook/assets/image%20%2818%29.png)

{% hint style="warning" %}
Note that the SSH configuration may/will change if you restart your VM. In this case you will need to re-configure \(re-run step 2 above\).
{% endhint %}

