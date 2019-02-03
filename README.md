Minecraft Server in Kubernetes
==============

This demonstrates how to run Minecraft in Kubernetes. It builds on the work of 
the [itzg Minecraft Server Docker Image](https://hub.docker.com/r/itzg/minecraft-server/) 
to create a docker container in kubernetes

This repository demonstrates how to run in a local kubernetes cluster using
local disks. 

How To Install
------

### Prerequisites 
* Have a Kubernetes cluster, be able to SSH to the nodes in your cluster
* Have your `kubeconfig` command line set up to talk to your cluster
* Have cluster admin privileges on this cluster
* Clone this repository to your computer

### Step 1: Create a directory for local files

SSH to one of the nodes in your kubernetes cluster. Create a directory where you want Minecraft files to exist.

```bash
ssh your_kubernetes_host
sudo mkdir -p "/mnt/local-volumes/minecraft"
sudo chmod 777 "/mnt/local-volumes/minecraft"
```

Get the kubernetes node name name of the host machine
on which you just added the directory.

Edit [20-volume-local.yaml](20-volume-local.yaml#L17). Update Line 17 to include the 
host path. Update line 25 to include the kubernetes node name

### Step 2: Configure your Minecraft Server

The [itzg Minecraft Server Docker Image](https://hub.docker.com/r/itzg/minecraft-server/) 
allows you to configure settings in the minecraft
server by setting environment variables. 

You can edit [30-deployment-local.yaml](30-deployment-local.yaml#L30) to add or
change environment variables for your purposes. See lines 30-35 for examples. See the minecraft-server docker image page for documentation on configuring your server.


### Step 3: Apply Kubernetes Configurations

Now, apply the configuration to Kubernetes

```bash
kubectl apply -f 00-namespace.yaml
kubectl apply -f 10-volume-claim-local.yaml
kubectl apply -f 20-volume-local.yaml
kubectl apply -f 30-deployment-local.yaml
kubectl apply -f 40-service-nodeport.yaml
```

### Step 4: Connect

We have created a node-port service, which means
that you can connect to any node in your kubernetes cluster on port `30565` (declared in [40-service-nodeport.yaml](40-service-nodeport.yaml#L12)) to attach to this minecraft server. Suppose that your kubernetes node has a public
ip address of 192.168.1.203.  When you configure your
minecraft server, connect to `192.168.1.203:30565`


Invitation to Help
------

Running on GKE: If anyone is interested in figuring this out the details to make this work
on Google Cloud GKE or Azure AKS, please send me a PR.

Helm Chart: If anyone wants to turn this into a Helm chart, that would be pretty cool.

