# Working with Docker Containers

**Introduction to Docker Containers**

Docker containers are lightweight, portable, and executable units that encapsulate an application and its dependencies. In the previous step, we worked a little with docker contaier. We would dive deep into the basics of working with Docker containers, from launching and running containers to managing their lifecycle.

**Running Containers**

To run a container, you use the "docker run' command followed by the name of the image you want to use.

Reacall that we pulled an ubuntu image from the official buntu repository on docker hub. Let's create a container from the ubuntu image. This command launches a container based on the Ubuntu image.

```bash
docker run ubuntu
```
![](1.%20docker%20run%20ubuntu.png)

Lets start the container. We can run the commander below:
```bash
docker start CONTAINER_ID
```
![](2.%20Docker%20start.png)

**Launching containers with different options**

Docker provides various options to customize the behavior of containers. For example, you can specify environment variables, map ports, and mount volumes. Here's an example of running a container with a specific environment variable:

```bash
docker run -e "MY_VARIABLE=my-value" ubuntu
```
![](3.%20docker%20run%20-e.png)

**Running container in the Background**

By default, containers run in the foreground, and the terminal is attached to the container's standard input/output. To run a container in the background, use the '-d" option:

```bash
docker run -d ubuntu
```

**Container Lifecycle**

Containers have a lifecycle that includes creating, starting, stopping, and restarting. Once a container is created, it can be started and stopped multiple times.

**Starting, Stopping and Restarting a Container**

To start a Container

```bash
docker start container_name
```
![](4.%20docker%20started.png)

**To stop a running container**
```bash
docker stop container_name
```
![](5.%20docker%20stopped.png)

**To restart a container**
```bash
docker restart container_name
```
![](6.%20docker%20restart.png)

**Removing Containers**

To remove container, You use `docker rm` command followed by the container ID or name.

```bash
docker rm container_name
```
![](7.%20docker%20remove.png)

This delete a container, but keep in mind that the associated image remains in your system.




