# Setting UP Minikube

## Container Orchestration with K8s

Kubernetes, often abbreviated as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications. It's designed to handle large-scale, distributed containerized applications, providing features like service discovery, load balancing, and storage orchestration. Essentially, Kubernetes acts as a platform for managing and orchestrating how your applications run across multiple machines, automating many operational tasks. 

Here's a more detailed breakdown:

**Container Orchestration:**

`Kubernetes` is a container orchestration tool. This means it manages containers, which are packages of software that include everything needed to run an application, ensuring they are deployed, scaled, and managed consistently across different environments.

**Automation:**

`Kubernetes` automates many common tasks related to container management, including deployment, scaling, and updates. 

**Scalability and Reliability:**

It allows you to easily scale your applications up or down based on demand and ensures they are highly available and fault-tolerant.

**Portability:**

`Kubernetes` enables you to run your applications anywhere – on-premises, in the cloud, or at the edge – with minimal changes. 

**Key Features:**

`Kubernetes` offers features like:

**Service Discovery and Load Balancing:** Automatically assigns IP addresses and DNS names to pods (groups of containers), enabling them to communicate with each other and distributes traffic effectively. 

**Storage Orchestration:**

Automatically manages storage for your applications, including mounting storage systems and managing persistent volumes. 

**Automated Rollouts and Rollbacks:**

Enables you to update your applications without downtime and easily revert to previous versions if needed. 

**Self-Healing:**

`Kubernetes` automatically restarts failed containers, replaces containers that don't meet health checks, and reschedules containers on healthy nodes. 

**Cluster:**

A `Kubernetes` cluster is a set of worker machines (nodes) that run containerized applications, managed by a master node and control plane. 

**Example:**

Imagine you have a website built with microservices. Kubernetes can help you deploy those microservices, ensure they are always running, scale them up during peak hours, and automatically update them with new versions without any downtime. 

### Components of Kubernetes  Cluster
![](1.%20Kub%20Cluster.png)

**Control Plane or Master Node:** The Control Plane, often referred to as the master node, is the brain of a Kubernetes cluster. It manages the entire cluster, making high-level decisions about the state of the system. Components like the API Server, Scheduler, Controller Manager, and etcd (key-value store for cluster data) constitute the Control Plane.

**Nodes:** Nodes are individual machines within a Kubernetes cluster responsible for running containerized applications. Each node is equipped with a container runtime (e.g., Docker), a kubelet (communicates with the master and manages containers on the node), and a kube-proxy (maintains network rules).
Nodes execute and manage containers, distribute workloads, and communicate with the control plane to maintain the desired state of the system. The collaboration of multiple nodes creates a scalable and resilient environment, forming the foundation for deploying and orchestrating containerized applications in Kubernetes.

**Pods:** Pods are the fundamental deployment units in Kubernetes, representing one or more closely related containers that share the same network namespace, storage, and set of specifications.
Containers within a Pod work together and are scheduled to run on the same node. Pods encapsulate the basic building blocks for deploying and scaling applications, fostering tight communication between co-located containers.

**Containers:** Containers in Kubernetes encapsulate and package applications, along with their dependencies and runtime environment, ensuring consistency across various computing environments.
Leveraging containerization technology, such as Docker, containers provide a lightweight, portable, and efficient way to deploy and run applications. In Kubernetes, containers are organized into Pods, the smallest deployable units.

![](2.%20K8s%20Master%20Node.png)

**API Server:** The API Server is the control plane component in Kubernetes that serves as the front end for the Kubernetes control plane. It exposes the Kubernetes API, allowing users, other components, and external entities to interact with the cluster, managing resources, and initiating various actions.

**Controller Manager:** The Controller Manager is a control plane component in Kubernetes responsible for maintaining the desired state of the cluster. It includes various controllers that watch the state of the cluster through the API Server and take corrective actions to ensure that the actual state aligns with the desired state.

**Scheduler:** The Scheduler is a control plane component in Kubernetes that assigns workloads to nodes in the cluster based on resource requirements, constraints, and availability. It plays a crucial role in distributing workloads evenly and efficiently across the worker nodes, optimizing resource utilization.

**etcd:** etcd is a distributed key-value store that acts as the cluster's single source of truth for all persistent cluster data. In Kubernetes, etcd is used to store configuration details, state information, and metadata, providing a reliable and consistent data store that ensures the integrity of the cluster's information.

**Kubelet:** The Kubelet is a vital component in Kubernetes running on each node in the cluster. It communicates with the Kubernetes control plane, specifically the API Server, to ensure that containers are running in a Pod as expected. Kubelet plays a key role in managing the containers on a node, handling tasks such as starting, stopping, and monitoring container processes based on the specifications received from the control plane.

**Kube Proxy:** Kube Proxy, or Kubernetes Proxy, is responsible for maintaining network rules on nodes. It enables communication between Pods and external entities, handling network routing and ensuring that the correct network policies are applied.

### Minikube
[Minikube Documentation](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download)

`Minikube` is an open-source tool that enables us to run Kubernetes clusters locally our machines. As we now know that kubernetes is a container orchestration platform that automates the deployment, scaling, and management of containerized applications. Minikube streamlines the creation of a local Kubernetes 1 environment, providing a user-friendly playground where we can safely build and test their applications before deploying them to a production setting.

**Getting Started with Minikube**
- Installing minikube on Windows
To install Minikube on Windows, we need to use [chocolatey](https://chocolatey.org/). Chocolatey, Just like linux `apt` and `yum`, is a windows package manager for installing, Updating and removing software packages on windows. 

i. Go to the search bar and launch a terminal with administartive access.

ii. Install Minikube
```bash
choco install minikube
```
![](3.%20Install%20Minikube.png)
Note: if you don't have [Chocolatey](https://chocolatey.org/), Installed follow the [official documentation](https://chocolatey.org/) to install it. 

iii. Minikube needs docker as a driver and also to pull its base image, therefore we need to install docker desktop for windows.

Go to [docker desktop](https://docs.docker.com/desktop/setup/install/windows-install/) official documentation to install it if not installed.

iv. Run the command below to start Minikube using docker as driver.

```bash
minikube start --driver=docker
```
![](4.%20Minikube%20start.png)

**Installing Minikube on Linux**

i. Launch a terminal with Administrative Access

ii. We need to install docker as a driver for Minikube and also for Minikube to pull base images for the K8s cluster.

```bash
sudo apt-get update
```
This is a Linux command that refreashes the Package list on a Debian based system, ensuring the latest software information is available for installation.
![](5.%20Sudo%20Update.png)

```bash
sudo apt-get install ca-certificates curl gnupg
```
This is a command that installs essentail packages including certificate authorities, a data transfer tool(curl) and GNU privacy Guard for secured communication and Package verification.
![](6.%20Install%20Certificate.png)

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```
This command creates a directory (etc/apt/ketrings) with spcific permission (0755) for storing keyring files, which are used for docker documentations
![](7.%20keyrings.png)

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
This command downloads the Docker GPG key using `curl`
![](8.%20Curl.png)

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
This command sets read permission for all users on the Docker GPG key file with the APT keyring directory.

**Let's add the repository to APT sources**

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

The "echo" command creates a Docker APT repository configuration entry for the Ubuntu system, incorporating the system architecture and Docker GPG key, and then "sudo tee /etc/apt/sources.list.d/docker.list > /dev/null" writes this configuration to the
*/ etc/apt/sources.list.d/ docker.list" file.
![](9.%20echo.png)

```bash
sudo apt-get update
```
Install the latest version of docker.
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
![](10.%20latest%20version%20of%20docker.png)

Verify that docker has been installed
```bash
sudo systemctl status docker
```
![](11.%20Verify%20Docker.png)

iii. Installing Minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
```
![](12.%20installing%20Minikube.png)

```bash
sudo dpkg -i minikube_latest_amd64.deb
```
The command above downloads Minikube's binary and install minikube using dpkg

![](13.%20dpkg.png)

iv. Starting Minikube
```bash
minikube start --driver=docker 
```
![](14.%20start%20minikube.png)

v. Kubectl is the command-line Interface (CLI) tool for interacting with and managing Kubernetes clusters, allowing users to deploy, inspect, and managing applications within the kubernetes environment. Lets install Kubectl.
```bash
sudo snap install kubectl --classic
```
![](15.%20Kubectl.png)
This command line will download the Kubernetes Command Line.

Installing on MAC




[Learn Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)

[Kubernetes Documentation](https://kubernetes.io/docs/home/)