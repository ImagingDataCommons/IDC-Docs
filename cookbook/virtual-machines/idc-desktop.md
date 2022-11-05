# 3D Slicer desktop VM

{% hint style="info" %}
In order to follow these instructions, you will need to have a project that has billing enabled. Please follow the instructions in [getting-started-with-gcp.md](../../introduction/getting-started-with-gcp.md "mention") to set up billing.&#x20;
{% endhint %}

## With a GPU

You can launch a VM with a GPU in your project with a command like this in your local terminal (replace `vm-name` with a name for your machine):

```
export VMNAME=vm-name
gcloud compute instances create ${VMNAME} \
  --machine-type=n1-standard-8 \
  --accelerator=type=nvidia-tesla-k80,count=1 \
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

Then you can open `http://localhost:6080/vnc.html?autoconnect=true` to get to your desktop.

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

On the remote machine run:
```
sudo systemctl stop novnc
sudo apt-get update
sudo apt-get -y install tigervnc-standalone-server websockify

vncserver -xstartup xfce4-session 
# here you will be prompted for a password for vnc

websockify --web /opt/novnc/noVNC/ 6080 localhost:5901 &

```

Then you can open `http://localhost:6080/vnc.html?autoconnect=true` to get to your desktop.

## Note 
This effort is a work in progress with a minimal desktop environment. Further refinement is expected and community contributions would be welcome! A description of the background and possible evolution of this work is [in this document](https://docs.google.com/document/d/1jfHqjS7Fer7Lhqea5bjyown0b04AsqeOhIBY2gWUDO4/edit).
