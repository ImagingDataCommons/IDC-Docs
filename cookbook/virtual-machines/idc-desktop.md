# 3D Slicer desktop VM

These instructions provide a reference example of how you can start up a traditional workstation desktop on a VM instance to run interactive applications like [3D Slicer](https://slicer.org) and access the desktop via a conventional web browser.  Two options are shown, either with or without a GPU.  Note that GPUs are significantly more expensive so only enable it if needed.  For 3D Slicer the main benefit of a GPU is for rendering, so operations like dicom processing and image segmentation are quite usable without a GPU.  Even volume rendering is fairly usable if you choose the CPU rendering option.  Other operations such as training machine learning models may benefit from an appropriate GPU.

A motivation for using desktop applications like 3D Slicer on a VM is that their computing power close to the data, so heavy network operations such as storage bucket or dicom store access may be significantly faster than accessing the same resources from a remote machine.  They are also highly configurable, so that you can easily allocate the number of cores or memory needed for a given task.  Note that can even change these configurations so that, for example, you can shut down the machine, add a GPU and more memory, and then boot the same instance and pick up where you left off.

In addition, these desktops are persistent in the sense that you can start a task such as labeling data for a machine learning task, disconnect your ssh session, and reconnect later to pick up where you left off without needing to restart applications or reload data.  This can be convenient when tending long-running computations, accessing your work from different computers, or working on a network that sometimes disconnects.

The instructions here are just a starting point.  There are many cloud options available to manage access scopes for the service accounts, allocate disks, and configure other options.

{% hint style="info" %}
In order to follow these instructions, you will need to have a project that has billing enabled. Please follow the instructions in [getting-started-with-gcp.md](../../introduction/getting-started-with-gcp.md "mention") to set up billing.&#x20;
{% endhint %}

Note that the disk images are based on Ubuntu 20.04 and are not regularly patched, so updating the OS right after booting is suggested.

If you run into an error updating the OS you may need to update the keys with this command: `sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com B53DC80D13EDEF05`.

## With a GPU

You can launch a VM with a GPU in your project with a command like this in your local terminal (e.g. replace `vm-name` with a name for your machine):

```
export VMNAME=vm-name
export PROJECT=project-name
export ZONE="use-east1-a"
gcloud compute instances create ${VMNAME} \
  --machine-type=n1-standard-8 \
  --accelerator=type=nvidia-tesla-k80,count=1 \
  --image-family=slicer \
  --image-project=idc-sandbox-000 \
  --boot-disk-size=200GB \
  --boot-disk-type=pd-balanced \
  --maintenance-policy=TERMINATE \
  --zone=${ZONE} \
  --project=${PROJECT}
```
Once it boots in about 90 seconds you can type:

```
gcloud compute ssh ${VMNAME} -- -L 6080:localhost:6080
```

Then you can open [http://localhost:6080/vnc.html?autoconnect=true](http://localhost:6080/vnc.html?autoconnect=true) to get to your desktop.

## Without a GPU

You can launch a VM without a GPU in your project with a command like this in your local terminal (replace `vm-name` with a name for your machine):

```
export VMNAME=vm-name
gcloud compute instances create ${VMNAME} \
  --machine-type=n1-standard-8 \
  --image-family=slicer \
  --image-project=idc-sandbox-000 \
  --boot-disk-size=200GB \
  --boot-disk-type=pd-balanced \
  --maintenance-policy=TERMINATE
```


Once it boots in about 90 seconds you can type:

```
gcloud compute ssh ${VMNAME} -- -L 6080:localhost:6080
```

On the remote machine run these commands once (first time you boot):
```
# these are on-time installs
sudo systemctl stop novnc
sudo apt-get update
sudo apt-get -y install tigervnc-standalone-server websockify
```

Each time you reboot the machine, run these commands:
```
vncserver -xstartup xfce4-session 
# here you will be prompted for a password for vnc if you haven't already
sudo systemctl stop novnc
nohup websockify --web /opt/novnc/noVNC/ 6080 localhost:5901 &
```


Then you can open [http://localhost:6080/vnc.html?autoconnect=true](http://localhost:6080/vnc.html?autoconnect=true) to get to your desktop.

## Note 
This effort is a work in progress with a minimal desktop environment. Further refinement is expected and community contributions would be welcome! A description of the background and possible evolution of this work is [in this document](https://docs.google.com/document/d/1jfHqjS7Fer7Lhqea5bjyown0b04AsqeOhIBY2gWUDO4/edit).
