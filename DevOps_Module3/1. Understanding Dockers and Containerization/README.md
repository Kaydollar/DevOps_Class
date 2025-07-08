# Introduction to Docker and Containers

## What are Containers:

In realm of software development and deployment, professionals used to face a dilemma. They crafted brilliant code on their local machines, only to find that when deployed to other environments, it sometimes does not work. The culprit? The notorious "it works on my machine" phenomenon.

Get started with `Docker`, a tool that emerged to a major problem IT industry. A man named Solomon Hykes, who, in 2013, unveiled Docker, a containerisation platform that promised to revolutionize the way IT professionals built, shipped, and ran applications.

Imagine containers as magical vessels that encapsulate everything an application needs to run smoothly
its code, libraries, dependencies, and even a dash of configuration magic. These containers ensure that an application remains consistent and behaves the same, whether it's running on a developer's laptop, a testing server, or a live production environment. Docker had bestowed upon IT professionals the power to say goodbye to the days of "it works on my machine."

![](1.%20Docker.jpg)



In software development, a container is a standardized, portable package that bundles an application's code, runtime, system libraries, and dependencies, allowing it to run consistently across different computing environments.

Think of it as a lightweight, self-contained package that includes everything an application needs to run, regardless of the underlying infrastructure. 

**Key features of containers:**

* **Portability:**

Containers can run on any infrastructure (desktops, servers, cloud) without modification, ensuring consistency across environments. 

* **Lightweight:**

They share the host operating system's kernel, making them more efficient and requiring less overhead than virtual machines. 

* **Isolation:**

Containers isolate applications from each other and the host system, enhancing security and preventing conflicts. 

* **Standardization:**

Containers offer a consistent way to package and deploy applications, simplifying development and operations. 

* **Agility:**

Containers facilitate faster development cycles and easier deployment, especially with microservices architectures. 

**Advantages of Containers**

* **Portability Across Different Environments:** In the past, deploying applications was add to navigating a treacherous maze, with compatibility issues lurking at every turn. Docker's containers, however, encapsulate the entire application along with its dependencies and configurations. This magical package ensures that your creation dances gracefully across different platforms, sparing you from the woes of the "it works on my machine" curse. With Docker, bid farewell to the headaches of environment mismatches and embrace a world where your application reigns supreme, irrespective of its hosting kingdom.

* **Resource Efficiency Compared to Virtual Machines:** Docker containers share the underlying host's operating system kernel, making them lightweight and nimble. This efficiency allows you to run multiple containers on a single host without the extravagant resource demands of traditional virtual machines.
Picture Docker containers as magical carriages, swiftly transporting your applications without burdening the kingdom with unnecessary excess. With Docker, revel in the harmony of resource optimization and application efficiency.

* **Rapid Application Deployment and Scaling:** Docker containers can be effortlessly spun up or torn down, facilitating the swift deployment of applications. Whether you're facing a sudden surge in demand or orchestrating a grand-scale production, Docker allows you to scale your applications seamlessly.
Imagine commanding an army of containers to conquer the peaks of user demand, only to gracefully retreat when the storm has passed. With Docker, the ability to scale becomes a wand in your hand, transforming the challenges of deployment into a choreographed ballet of efficiency.

**Comparison of Docker Container with Virtual Machines**

     Docker and virtual machines (VMs) are both technalogies used for application deployment, but they differ in their approach to virtualization. Virtual machines emulate entire operating systems, resulting in higher resource overhead and slower performance. In contrast, Docker utilizes containerization, encapsulating applications and their dependencies while sharing the host OS's kernel. This lightweight approach reduces resource consumption, provides faster startup times, and ensures portability across different environments. Docker's emphasis on microservices and standardized packaging fosters scalability and efficiency, making it a preferred choice for modern, agile application development, whereas virtual machines excel in scenarios requiring stronger isolation but at the cost of increased resource usage. The choice between Docker and VMs depends on specific use cases and the desired balance between performance and isolation.


**Containers vs. Virtual Machines (VMs):**

While both are used for virtualization, containers are more lightweight and share the host OS kernel, whereas VMs include a full guest OS, making them heavier and requiring more resources. 

**Benefits of using containers:**

* **Faster deployments:** Containers enable quicker and more efficient deployments of applications. 

* **Resource optimization:** They allow for better utilization of computing resources due to their lightweight nature. 

* **Increased portability:** Applications can be moved easily between different environments. 

* **Improved consistency:** Containers ensure that applications behave the same way across different environments. 

* **Enhanced security:** Isolation within containers helps protect applications from security threats. 

**Examples of containerization technologies:**

* **Docker:** A popular platform for building, sharing, and running containers. 

* **Kubernetes:** An open-source platform for automating containerized application deployment, scaling, and management. 

**Importance of Docker**

* **Technology and Industry Impact:** The significance of Docker in the technology landscape cannot be overstated. Docker and containerization have revolutionized software development, deployment, and management. The ability to package applications and their dependencies into lightweight, portable containers addresses key challenges in software development, such as consistency across different environments and efficient resource utilization.

* **Real-World Impact:** Implementing Docker brings tangible benefits to organizations. It streamlines the development process, promotes collaboration between development and operations teams, and accelerates the delivery of applications. Docker's containerization technology enhances scalability. facilitates rapid deployment, and ensures the consistency of applications across diverse environments.
This not only saves time and resources but also contributes to a more resilient and agile faftware development lifecycle.

**Project Goals**

**By the end of this course, learners should aim to achieve the following:**

1. Grasp the concept of containers, their isolation, and their role in packaging applications.

2. Familiarize themselves with key Docker features, commands, and best practices.

3. Comprehend how Docker containers contribute to resource efficiency compared to traditional virtual machines.

4. Learn how Docker ensures consistent application behavior across different development, testing, and production environments.

5. Master the techniques for quickly deploying and scaling applications using Docker.

### Getting Started with Docker

**Installing Docker**

We need to launch an ubuntu 20.04 LTS instance and connect to it, then follow the steps below

![](2.%20Ubuntu%20ssh%20login.png)

Before installing Docker Engine for the first time on a new host machine, it is necessary to configure the Docker repository. Following this setup, we can proceed to install and update Docker directly from the repository.

**Now let's first add docker's oficial GPG key**

Yoy can learn more about [GPG keys](https://help.ubuntu.com/community/GnuPrivacyGuardHowto)

```bash
sudo apt-get update
```

![Sudo apt-get update](3.%20Sudo%20apt-get%20update.png)

This is a linux command that refreashes the packagelist on a Debian-Based system, ensuring the latest software information is available for installation.


```bash
sudo apt-get install ca-certificates curl gnupg
```
![sudo apt-get install ca-certificates curl gnupg](4.%20sudo%20apt-get%20install%20ca-certificates%20curl%20gnupg.png)

This is a linux command that installs essentials package certificate authorities, a data transfer tool(curl), and the GNU Privacy Guard for secure communication and Package Verification. 

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

![sudo install -m 0755 -d /etc/apt/keyrings](5.%20sudo%20install%20m%200755%20d%20etcapt%20keyrings.png)

The command above creates a directory (/ etc/apt/keyrings) with specific permissions (0755) for storing keyring files, which are used for docker's authentication

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
![](6.%20docker.png)

This downloads the Docker GPG key using `curl`

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
![read permission](7.%20read%20Permission.png)

Sets read permission for all users on the Docker GPG key within the APT keyring directory.

**Lets add the repository to Apt resources**

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
![](8.%20docker.png)

The "echo" command creates a Docker APT repository configuration entry for the Ubuntu system, incorporating the system architecture and Docker GPG key, and then "sudo tee /etc/apt/sources.list.d/dockerlist > /dev/null" writes this configuration to the
*/ etc/apt/sources.list.d/docker.list" file.

```bash
sudo apt-get update
```
Update Package. 

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
![Latest-version_of_docker](9.%20Install%20docker.png)
Installing Latest Version of Docker

* Verify that Docker has been installed.

```bash
sudo systemctl status docker
```
![Verify_docker](10.%20Verify%20docker.png)

By default, After installing docker, it can only be run by root user or using `sudo` command. To run the docker command without sudo. 

```bash
sudo usermod -aG docker ubuntu
```
with this command we can execute the docker commands with using sudo

![](11.%20Super_user.png)

**Running the 'Hello World' Container**

Using the `docker run` command.

The 'docker run' command is the entry point to execute containers in Docker. It allows you to create ar start a container based on a specified Docker image. The most straightforward example is the "Hello World" container, a minimalistic container that prints a greeting message when executed

```bash
# Run the "Hello World" container
docker run hello-world
```
!["hello_world"](12.%20docker%20run%20Hello%20World.png)

When you execute this command, Docker performs the following steps:

1. **Pulls Image (if not available locally):** Docker checks if the "hello-world' image is available locally. not, it automatically pulls it from the Docker Hub, a centralized repository for Docker images.
2. **Creates a Container:** Docker creates a container based on the "hello-world' image. This containe is an instance of the image, with its own isolated filesystem and runtime environment.
3. **Starts the Container:** The container is started, and it executes the predefined command in the
*hello-world image, which prints a friendly message.

**Understanding the Docker Image and Container Lifecycle**

**Docker Image:** A Docker image is a lightweight, standalone, and executable package that includes everything needed to run a piece of software, including the code, runtime, libraries, and system tools.
Images are immutable, meaning.they cannot be modified once created. Changes result in the cred ion of a new image.

- **Container Lifecycle:** Containers are running instances of Docker images.
    - They have a lifecycle: `create, start, stop, and delete`

    - Once a container is created from an image, it can be started, stopped, and restarted.

**Verifying the Successful Execution**

You can check if the images is now in your local environment with Example Output:

```bash
docker images
```
![docker_images](13.%20docker%20images.png)

If you encounter any issues, ensure that Docker is properly installed and that your user has the necessary permissions to run Docker commands.

This simple "Hello World" example serves as a basic introduction to running containers with Docker. It helps verify that your Docker environment is set up correctly and provides insight into the image and container lifecycle. As you progress in this course, you'll explore more complex scenarios and leverage Docker for building, deploying, and managing diverse applications.

### Basic docker commands

Docker run 

The docker run command is fundamental for executing containers. It creates and starts a container based on a specified image.

```bash
# Run a container based on the "nginx" image
docker run hello-world
```
![docker_run](14.%20docker%20run.png)

This example pulls the "ngin" image from Docker Hub (if not available locally) and starts a container using that image.

**This example pulls the "ngin" image from Docker Hub (if not available locally) and starts a container using that image.

**Docker PS**

The "docker ps' command displays a list of running containers. This is useful for monitoring active containers and obtaining information such as container IDs, names, and status.
![docker_ps](15.%20docker%20ps.png)

To view all containers, including those that have stopped, add the `-a` option.
```bash
# List all containers (running and stopped)
docker ps -a
```
![docker_ps_ -a](16.%20docker%20ps%20-a.png)

**Docker stop**

The `docker stop` command halt a running container.
![docker_stop](17.%20docker%20stop.png)

**Docker pull**

The `docker pull` command downloads a docker image from a registry, such as docker hub, to your local computer.
![docker pull](18.%20Docker%20pull.png)

**Docker Push**

The `docker push` command upload a local Docker image to a registry, making it available for others to pull

Ensure you have logged in to Docker Hub using `docker login` before pushing image. 

**Docker Images**

The `docker images` commands list all locally available Docker images.

![docker images](19.%20docker%20images.png)

**Docker DMI**

The `docker dmi** removes one or more images from the local machine.

![docker rmi](20.%20docker%20rmi.png)

