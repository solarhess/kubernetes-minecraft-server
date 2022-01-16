Minecraft Server in Kubernetes
==============

This demonstrates how to run Minecraft in Kubernetes. It builds on the work of 
the [itzg Minecraft Server Docker Image](https://hub.docker.com/r/itzg/minecraft-server/) 
to create a docker container in kubernetes.

We also use Helm to make installation, and customization, a breeze.

This repository demonstrates how to run in a local kubernetes cluster using
local disks. 

How To Install
------

### Prerequisites 
* Have a Kubernetes cluster, be able to SSH to the nodes in your cluster
* Have Helm installed
* Have your `kubeconfig` command line set up to talk to your cluster
* Have cluster admin privileges on this cluster
* Clone this repository to your computer

### Step 1: Create a directory for local files

SSH to one of the nodes in your kubernetes cluster. Create a directory where you want Minecraft files to exist.

Example:

```bash
ssh your_kubernetes_host
sudo mkdir -p "/mnt/local-volumes/minecraft"
sudo chmod 777 "/mnt/local-volumes/minecraft"
```

Get the kubernetes node name name of the host machine
on which you just added the directory.

Edit [values.yaml](helm/minecraft/values.yaml#L7). Update Line 7 to your node's hostname.
If you used a different path for your worldfile, update volume.path in [values.yaml](helm/minecraft/values.yaml#L17).

### Step 2: Configure your Minecraft Server

The [itzg Minecraft Server Docker Image](https://hub.docker.com/r/itzg/minecraft-server/) 
allows you to configure settings in the minecraft
server by setting environment variables. 

You can edit [helm/minecraft/deployment.yaml](helm/minecraft/deployment.yaml#L1) to add or
change environment variables for your purposes. See lines 30-35 for examples. See the minecraft-server docker image page for documentation on configuring your server.

We've included just a couple of the most common configurations in [values.yaml](helm/minecraft/values.yaml#L10) like gamemode, message of the day, and port.


### Step 3: Helm install

Now, apply the configuration to Kubernetes. `cd` to the project root and run:

```bash
helm install minecraft --generate-name
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

