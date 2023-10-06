# ACCESS allocations

[Advanced Cyberinfrastructure Coordination Ecosystem (ACCESS)](https://access-ci.org/) is a program supported by the US National Science Foundation (NSF) to provide educators with **free** and convenient access to advanced computational resources.

If you have a university email account, you can complete a relatively easy application process to receive an allocation of free credits that you can then use to create pre-configured GPU-enabled cloud-based linux virtual machines with desktop interface available via browser. You can use those machines, for example, to have a convenient access to an instance of [3D Slicer](https://www.slicer.org/) for experimenting with AI models, or for training DL networks.

<figure><img src="../.gitbook/assets/image.png" alt="" width="319"><figcaption><p>Example of the configurations available for the Ubuntu 22.04 base image</p></figcaption></figure>

## How to get started

Follow these steps:

1. Create an account and request an ACCESS allocation at this page: [https://docs.jetstream-cloud.org/alloc/overview/](https://docs.jetstream-cloud.org/alloc/overview/). There are 4 different levels, with each giving you a different number of “credits” that you use to create your VM instances. Each of these levels requires you to submit a different application. For the Explore ACCESS allocation (lowest tier), you need to write a simple abstract to justify why we needed these resources. Other tiers require more lengthy descriptions of what you’ll do with the ACCESS resources. In our experience, applications can be approved in as soon as I a few days after submitting the application. You can be a PI and have multiple Co-PIs with you on the project, so you can all access the Jetstream2 resources.
2. Once you get approved, your allocation is valid for a 12 month period, and you get half of the credits to start. To start using these credits you exchange them for Service Units (SUs) here: [https://docs.jetstream-cloud.org/general/access/](https://docs.jetstream-cloud.org/general/access/). Usually this exchange is approved within a few days if not less.
3. Once you get the SU’s you can access JetStream interface to configure and create VMs here: [https://jetstream2.exosphere.app/](https://jetstream2.exosphere.app/) (you can lean more about available configurations from this documentation page: [https://docs.jetstream-cloud.org/general/vmsizes/](https://docs.jetstream-cloud.org/general/vmsizes/)).
4. Once you created a VM and your setup is complete, it’s very easy to connect to your VMs through ssh or Web desktop interface.&#x20;

## Why we recommend ACCESS allocations

* **It is free to educators!**
* **Very easy to set up.** As of writing, there is no similar product available from Google Cloud, which would provide desktop access to a VM with a comparable ease of access. AWS provides [AppStream2](https://aws.amazon.com/appstream2/), but we have yet to experiment to evaluate it.
* **You can do a lot with the basic credit allocation!** Entry-level allocations can be on the order of 100,000s, while the burn rate is, for example, 8 SUs/hour for a medium size VM (8 CPUs/30 GB RAM). As a reference, it takes about 1 hour to build Slicer application from scratch on such a VM using 7 threads.
* **Geared to help you save!** Unlike the VMs you get from the commercial providers, JetStream VMs can be _shelved._ Once a VM is shelved, you spend 0 SUs for keeping it around (in comparison, you will keep paying for the disk storage of your GCP VMs even when they are turned off).
* **Customer support is excellent!** We received responses within 1-2 days. On some occasions, we observed glitches with Web Desktop, but those could often be resolved by restarting the sequence.
