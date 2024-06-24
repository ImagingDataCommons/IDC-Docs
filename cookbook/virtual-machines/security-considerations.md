# Security considerations

### Computing on the Cloud

Most of the same linux commands, scripts, pipelines/workflows, imaging software packages and docker containers that you run on your local machine can be executed on virtual machines on Google Cloud with experimentation and fine tuning.

1. The basics and best practices on how to launch virtual machines (VMs) are described [here](https://docs.google.com/document/d/1U3JalN711lNcuGd\_In8T89uGmaJTj480OladQaOPdOk/edit) in our documentation. NOTE: When launching VMs, please maintain the default firewall settings.
2. Compute Engine instances can run the public images for Linux and Windows Server that Google provides as well as private custom images that you can [create](https://cloud.google.com/compute/docs/images/create-delete-deprecate-private-images) or [import from your existing systems](https://cloud.google.com/compute/docs/images/importing-virtual-disks).\
   \
   Be careful as you spin up a machine, as larger machines cost you more. If you are not using a machine, shut it down. You can always restart it easily when you need it.\
   \
   Example use-case: You would like to run Windows-only genomics software package on the TCGA data. You can create a Windows based VM instance.
3. More details on how to deploy docker containers on VMs are described here in Google’s documentation: [deploying containers](https://cloud.google.com/compute/docs/containers/deploying-containers)
4. A good way to estimate costs for running a workflow/pipeline on large data sets is to test them first on a small subset of data.
5. There are different VM types depending on the sort of jobs you wish to execute. By default, when you create a VM instance, it remains active until you either stop it or delete it. The costs associated with VM instances are detailed here: [compute pricing](https://cloud.google.com/compute/pricing)
6. If you plan on running many short compute-intensive jobs (for example indexing and sorting thousands of large bam files), you can execute your jobs on preemptible virtual machines. They are 80% cheaper than regular instances. [preemptible vms](https://cloud.google.com/preemptible-vms/)

Example use-cases:

* Using preemptible VMs, researchers were able to quantify transcript levels on over 11K TGCA RNAseq samples for a total cost of $1,065.49.\
  Tatlow PJ, Piccolo SR. [A cloud-based workflow to quantify transcript-expression levels in public cancer compendia](https://www.nature.com/articles/srep39259). Scientific Reports 6, 39259
* Also Broad’s popular variant caller pipeline, GATK, was designed to be able to run on preemptible VMs.
* Google cloud computing can be estimated [here](https://cloud.google.com/compute/all-pricing).

### Be Very Careful with Tokens containing passwords.  They should NOT be moved to Github

Because of the ability to see a [history](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/viewing-and-comparing-commits/differences-between-commit-views) of Github postings, if a password or bearer token is part of software code (e.g. notebook or colaboratory) it will be permanently available on Github.  This is a security risk!!  Do not put bearer tokens or other passwords into workbooks, instead refer to them in the code and place those in a location not posted into Github (if you do post it to GitHub, it then immediately becomes public, usable, and able to be stolen and used maliciously by others).  If you do accidentally post one to Github: 1) immediately change passwords on your systems to remove the exposure provided by the exposed password, 2) let those who involved in the security of your system and data know, and 3) remedy your code-base so future saves to Github do not include passwords or tokens in your codebase.

### Storage on the Cloud

The Google Cloud Platform offers a number of different storage options for your virtual machine instances: [disks](https://cloud.google.com/compute/docs/disks/)

1. [Block Storage:](https://cloud.google.com/compute/docs/disks/#pdspecs)

* By default, each virtual machine instance has a single boot persistent disk that contains the operating system. The default size is 10GB but can be adjusted up to 64TB in size. (Be careful! High costs here, spend wisely!)
* Persistent disks are restricted to the zone where your instance is located.
* Use persistent disks if you are running analyses that require low latency and high-throughput.

2. [Object Storage:](https://cloud.google.com/compute/docs/disks/#gcsbuckets) Google Cloud Storage (GCS) buckets are the most flexible and economical storage option.

* Unlike persistent disks, Cloud Storage buckets are not restricted to the zone where your instance is located.
* Additionally, you can read and write data to a bucket from multiple instances simultaneously.
* You can mount a GCS bucket to your VM instance when latency is not a priority or when you need to share data easily between multiple instances or zones.\
  An example use-case: You want to slice thousands of bam files and save the resulting slices to share with a collaborator who has instances in another zone to use for downstream statistical analyses.

You can save objects to GCS buckets including images, videos, blobs and unstructured data.\
A comparison table detailing the current pricing of Google’s storage options can be found here: [storage features](https://cloud.google.com/storage/features/)
