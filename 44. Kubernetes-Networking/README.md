# Networking in Kubernetes
Networking refers to the mechanism and configurations that allow communication between different components (pods, service, and other resources) within a Kubernetes cluster.

Kubernetes provides a flexible and powerful networking model to enable seamless interaction between containers and services, whether they are running on the same node or across different nodes in a cluster.

## Some key aspects of networking in Kubernetes
**Pod Networking:** Containers within a pod share the same network namspace, allowing them to communicate with each other using localhost. This enables tight coupling between containers within the same pod.

**Service Networking:** Kubernetes Network Policies define rules for controlling the communication between pods. These policies allow administrators to specify how pods can communicate with each other, enhancing security within the cluster.

**Container Network Interface (CNI):** Kubernetes relies Container Network Interface to implement networking solutions. CNIs provide a standardized interface for networking plugins to integrate with Kubernetes, allowing for flexibility and choice in networking implementations.

Let's get our hands on pod networking in Kubernetes by deploying a pod with multiple containers, showcasing how they share the same network namespace and can communicate with each other using localhost. Here's a step-by-step guild using Kubernetes and kubectl.

1. Create a Multi-Container Pod YAML file (e.g., `multi-container-pod.yaml`): and paste the snippet below
```bash
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: container-1
    image: nginx:alpine
  - name: container-2
    image: busybox
    command:
      - '/bin/sh'
      - '-c'
      - 'mkdir -p /usr/share/nginx/html && while true; do echo "Hello from Container 2" >> /usr/share/nginx/html/index.html; sleep 10; done'
```
![](1.%20File%20create.png)

**Explanation of the YAML snippet above:** apiVersion: v1: Specifies the Kubernetes API version for the object being created, in this case, a Pod.

**kind:** Pod: Defines the type of Kubernetes resource being created, which is a Pod. pods are smallest deployable units in Kubernetes and can host one or more containers.

**metedata:** Conatains metedata for the Pod, including the name of the Pod, which is set to "multi-container-pod".

**spec:** Describe the desired state of the Pod.

**containers:** Specifies the containers configuration for the Pod.

- **name: container-1:** Define the first container in the Pod with the name "container-1" and uses the nginx:alpine image.

- **name: container-2:** Defines the second container in the Pod with the name "container-2" and uses the busybox image. Additionally, it specfies a command to create on HTML file in the Nginx directory and continuously appends "Hello from the Container 2" to it every 10 seconds.

The pod has two containers - one running the Nginx web server and another running BusyBox with a simple command to continuously append "hello from Container 2" to the Nginx default HTML file.

2. Apply the Pod Configuration:

```bash
kubectl apply -f multi-container-pod.yaml
```
![](2.%20apply%20container.png)

3. Check Pods Status and Logs:
```bash
kubectl get pods
kubectl logs multi-container-pod -c container-1
kubectl logs multi-container-pod -c container-2
```
![](3.%20Containers.png)

You'll observe the both containers are running within the same pod, and they share the same network namespace. The Nginx container server its default page, and the BusyBox container continuously updates the HTML file. Since Kubernetes kubectl logs reads from stdout and stderr, you're not seeing anything because the script’s output is redirected to `/usr/share/nginx/html/index.html instead`.

4. **Access Nginx BusyBox Container:**

```bash
kubectl exec -it multi-container-pod -c container-2 -- /bin/sh
```
Then after I finally exec into the container I run this command below.
```bash
cd /usr/share/nginx/html

ls 

cat index.html
```
![](4.%20index.png)

**Now, within the BusyBox container, I can use tools like curl or wget to access `http://localhost` and see the Nginx page.**

```bash
wget -qO- http://localhost
```
I am using wget because curl is not install on the container and the `-q` means to be quiet and `-O-` means for stdout.
![](5.%20nginx.png)


