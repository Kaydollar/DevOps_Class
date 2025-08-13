# Working with Kubernetes Resources
## Introduction To YAML
A Kubernetes YAML file is a text file written in YAML syntax that describes and defines Kubernetes resources. YAML is a human-readable data serialization format that is commonly used for configuration files.

In the context of Kubernetes, these YAML files serve as a declarative way to specify the desired state of the resources such as pods, container, service and deployment you want to deploy and manage within a Kubernetes cluster.

## Basic Structure Of YAML File
YAML uses indentation to represent the hierarchy of data, and it uses whitespace (usually spaces, not tabs) for indentation

```bash
key1: value1
key2:
  subkey1: subvalue1
  subkey2: subvalue2
key3:
  - item1
  - item2
```
### Data Types
**Scalar:** scalar are single values.

- Strings
```bash
name: John Doe
```

- Numbers:
```bash
age: 25
```

- Booleans
```bash
is_student: true
```

### Collections
- List (arrays):
```bash
fruits:
  - apple
  - banana
  - orange
````

- Maps(Key-value pairs):
```bash
person:
  name: Alice
  age: 30
```

### Nested Structures YAML allows nesting of structures:
```bash
employee:
  name: John Doe
  position: developer
  skills:
    - Python
    - JavaScript
```

### Comments
- In YAML, comments starts with `#`
```bash
# This is a comment
key: value
```

### Multiline Strings
Multiline Strings can be represented using the `|` or `>` characters:
```bash
description: |
  This is a multiline
  string in YAML.
```

- Anchors and Aliases:

You can use `&` to create an anchor and `*` to create alias:
```bash
first: &name John
second: *name
```
So this is more like setting a variables. In this example, second has the same value as first.

Now that we've got the basic, practice and reading YAML to become confortable with its syntax. It's widely used in configuration files for various tools and systems.

# Deploying Applications in Kubernetes

In Kubernetes, deploying applications is a fundemental skills. Deployment involves the process of taking your application code and running it on a Kubernetes cluster, ensuring that it scales, manages resource efficiently, and stays resilient. This hand-on project will guild me through deploying application using Minikube, a lightweight, single-node Kubernetes cluster perfect for development or testing.

## Deployments in Kubernetes

In Kubernetes, a **Deployment is a declarative approach to managing and scaling applications. It provides a blueprint for the desired state of the application, allowing Kubernetes to handle the complexities of deploying and managing replicas. Whether you're running a sinple web serber or a more complex microservices architecture, Deployment are the cornerstone for maintaining application consistency and availability.

## Service in Kubernetes

Once the application is deployed, it needs a way to be accessed by other perts of the system or external users. This is where Service comes into play. In Kubernetes, a Service is an abstraction that defines a logical set of Pods and a policy by which to access them. It acts as a stable endpoint to connect to the application, allowing for easy communication within the cluster or external sources. Some of the several types of Services in Kubernets;

- **ClusterIP:** Purpose: The default type. Expose the service in a cluster internal IP. Accessible only within the cluster.

- **NodePort:** Exposes the Service on each Node's IP at a static port (NodePort). Accessible externally using NodeIP.

- **LoadBalancer:** Exposes the Service externally using a cloud provider's load balancer. Accessible externally through the load balancer's IP.

In subsequent sections. I will dive deep into deployment strategies and service configuration within the kubernetes ecosystem, delving into the intricacies of these componenets to ensure a thorough understanding and proficiency in their utilization.

**Deploying a Minikube Sample Application:** Using YAML files for deployments and services in Kubernetes is like cratfting a detailed plan of the application, while direct deployment with kubectl commands is more like giving quick, on-the-spot intructions to launch and manage the application. Let's create a minikube deployment and service with kubectl.

```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
```
![](1.%20Creating%20Pods.png)

The command above creates a Kubernetes Deployment named "hello-minikube" running the "kicbase/echo-server:1.0" container image

```bash
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```
The command above exposes the Kubernetes Deployment named "hello-minikube" as a NodePort service on port 8080.
![](2.%20Expose.png)

```bash
kubectl get services hello-minikube
```
![](3.%20Get%20service.png)

The easiest way to acccess this service is to let minikube launch a web browser for you

```bash
minikube service hello-minikube 
```
![](4.%20Minikube%20service.png)

**Note:** If the web browser can't be open because of no GUI on my environment. So what I will do is to tunnel the minikube port to another VM that has GUI installed. Minikube uses a private network interface, and when you forward a port (via SSH tunnel or minikube service), it binds to localhost (127.0.0.1) by default — not the VM's IP address. This is possible by using putty SSH TUNNEL

# Working With YAML Files
I already have an image push to dockerhub. Now we are going to reuse the image in our YAMl script for deployment.

**Steps:**

1. Create a new folder I will use for this project which I named my-nginx-yaml and also navigate to the directory.
```bash
mkdir my-nginx-yaml
cd my-nginx-yaml
```
![](5.%20mkdir.png)

2. Then I created a new file inside the directory I created in the first step, which is name `nginx-deployment.yaml`

paste the content below
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      labels:
        app: my-nginx
    spec:
      containers:
      - name: my-nginx
        image: dareyregistry/my-nginx:1.0
        ports:
        - containerPort: 80
```

![](6.%20yaml%20file.png)

The provided YAML snippet defines a Kubernetes Deployment for deploying an instance of the Nginx web server. Let's break down the key components:

**apiVersion YAML/vi:** Specifies the Kubernetes API version for the object being created, in this case, a Deployment in the "apps" group.

**kind:** Deployment: Defines the type of Kubernetes resource being created, which is a Deployment. Deployment are used to manage the deployment and scaling of applications.

**metedata:** Contains metedatas for Deployment, including the name of the Deployment, which is set to "my-nginx-deployment."

**spec:** Describe the desired state of the Deployment.

**replicas:** 1: Specifies that the desired number of replicas (instances) of the Pods controlled by this Deployment is 1.

**selector:** Defines how the Deployment selects which Pods to manage. In this case. it uses the label "app:my-nginx" to match Pods.

**templete:** Specfies the templete for creating new Pods.

**metedata:** Contains label for the Pods, and in this case, the label is set to "app: my-nginx".

**spec:** Describe the Pods specification.

containers: Defines the containers within the Pod.

**name:** my-nginx: Sets the name of the container to "my-nginx".

**image:** dareyregistry/my-nginx:1.0: Specifies the Docker images to be used for the Nginx container. The container. The image is "ridwanaz/my-nginx" with version "1.0".

**ports:** Specifies the port mapping for the conntainer, and in this case, it expose port 80.

3. Created a new file which I called **nginx-service.yaml** and paste the content below
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-nginx-service
spec:
  selector:
    app: my-nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
  ```

![](7.%20yaml%20file.png)

The provided YAML snippet defines a Kubernetes Service for exposing the Nginx application to the external world. Let's break down the key components:

**apiVersion: v1:** Specifies the Kubernetes API version for the object being created, in this case, a Service.

**kind: Service:** Define the type of Kubernetes resource being created, which is a Service. Service provide n a stable endpoint for accessing a set of Pods.

**metedata:** Contains metedata for the Service, including the name of the Service, which is set to "my-nginc-service".

**spec:** Describe the desired state of the Service.

**selector:** Specifies the labels used to select which Pods the Service will route traffic to. In this case, it selects Pods with the label "app: my-nginx".

**ports:** Specifies the ports configuration for the Service.

**protocol: TCP:** Specifies the transport layer protocol, which is TCP in this case.

**port: 80:** Defines the port on which the Service will exposed.

**targetPort: 80:** Specifies the port on the Pods to which the traffic will be forwarded.

**type: NodePort:** Sets the type of the Service to NodePort. This means that the Service will be accessiblr externally on each Node's IP address at a static port, which will be automatically assigned unless specified.

4. After defining both my deployment and service YAML file then I run the command below.

```bash
kubectl apply -f nginx-deployment.yaml

kubectl apply -f nginx-service.yaml
```

5. Verify the deployment

```bash
kubectl get deployments

kubectl get services
```
![](8.%20Completed.png)

6. Then I access my deployment on the web browser.

```bash
minikube service my-nginx-service --url
```
![](9.%20URL.png)

7. Then I opened my web browser and input this URL http://127.0.0.1:64090/.

![](Accessed.png)
