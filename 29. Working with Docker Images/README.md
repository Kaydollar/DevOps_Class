# Working with Docker Images

## Introduction to Docker Images

Docker images are read-only templates that contain instructions for creating a container. A Docker image is a snapshot or blueprint of the libraries and dependencies required inside a container for an application to run.

Docker images are the building blocks of containers. They are lightweight, portable, and self-sufficien packages that contain everything needed to run a software application, including the code, runtime libraries, and system tools. Images are created from a set of instructions known as a Dockerfile, which specifies the environment and configuration for the application.

**Pulling Images from Docker Hub**

<u>Docker Hub</u> is a cloud-based registry that hosts a vast collection of Docker images. You can pull image from Docker Hub to your local machine using the `docker pull` command

**To explore available images on Docker Hub, the docker command provides a search subcommand. For instance, to find the Ubuntu image, you çan execute:**

```bash
docker search ubuntu
```
This command allows you to discover and explore various images hosted on Docker Hub by providing relevant search results. In this case, the output below. 

![Docker search ubuntu](1.%20Docker%20search%20ubuntu.png)

In the "OFFICIAL" column, the "OK designation signifies that an image has been constructed and is officially supported by the organization responsible for the project. Once you have identified the desired image, you can retrieve it to your local machine using the "pull" subcommand.

**To download the official Ubuntu image to your computer, use the following command:**

```bash
docker pull ubuntu
```
Executing this command will fetch the ubuntu image from the Docker Hub and store it locally on your machine, making it ready for use in the creating containers. 

![Docker pull ubuntu](2.%20Docker%20pull%20ubuntu.png)

Once an image has been successfully downloaded, you can proceed to run a container using that downloaded image by employing the "run" subcommand. Similar to the hello-world example, if an image is not present locy when the "docker run' subcommand is invoked, Docker will automatically download l the required image before initiating the container.

To view the list of images that have been downloaded and are available on your local machine, enter the following command:

```bash
docker images
```
Executing this command provides a comprehensive list of all the images stored locally, allowing you to verify the presence of the downloaded image and gather information about its size, version, and other relevant details.

![docker images](3.%20Docker%20images.png)

**Dockerfile**

A Dockerfile is a plaintext configuration file that contains a set of instructions for building a Docker image.
It serves as a blueprint for creating a reproducible and consistent environment for your application.
Dockerfiles are fundamental to the containerization process, allowing you to define the steps to assemble an image that encapsulates your application and its dependencies.

**Creating a Dockerfile**

In this dockerfile file, we will be using an nginx image. `Nginx` is an open source software for web serving. reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. It is recommended you read more on <u>[Nginx here](https://www.f5.com/glossary/nginx)</u>

To create a Dockerfile, use a text editor of your choice, such as vim or nano. Start by specifying a base image, defining the working directory, copying files, installing dependencies, and configuring the runtime environment.

Here's a simple example of a Dockerfile for a html file: Let's create an image with using a dockerfile. Paste the code snippet below in a file named `dockerfile` This example assumes you have a basic HTML file named `index.html` in the same directory as your Dockerfile.

```bash
# Use the official NGINX base image
FROM nginx:latest

# Set the working directory in the container
WORKDIR  /usr/share/nginx/html/

# Copy the local HTML file to the NGINX default public directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 to allow external access
EXPOSE 80

# No need for CMD as NGINX image comes with a default CMD to start the server
```
1. **FROM nginx:latest:** Specifies the official NGINX base image from Docker Hub.
2. **WORKDIR /usr/share/nginx/html/:** Specifies the working directory in the container
3. **COPY index.html /usr/share/nginx/html/:** Copies the local `index.html` file to the NGINX default public directory, which is where NGINX serves static content from.
4. **EXPOSE 80:** norms Docker that the NGINX server will use port 80. This is a documentation feature and doesn't actually publish the port.
5. **CMD:** NGINX images come with a default CMD to start the server, so there's no need to specify it explicitly.

HTML file named `index.html` in the same directory as your dockerfile.

**NB:**
1. Create a Project Folder
In your terminal (Git Bash, Linux terminal, or VS Code terminal):
```bash
mkdir my-html-site
cd my-html-site
```
![](4.%20mk%20and%20cd%20my-html-site.png)
2. Create the index.html File
```bash
vim index.html
```
Paste this content into the terminal:
```bash
<!DOCTYPE html>
<html>
<head>
    <title>Hello from Docker</title>
</head>
<body>
    <h1>🚀 Hello! This page is served from inside a Docker container.</h1>
</body>
</html>
```
3. Create the Dockerfile

Now run:
```bash
nano Dockerfile
```
Paste this content:
```bash
# Use the official NGINX base image
FROM nginx:latest

# Set the working directory in the container
WORKDIR  /usr/share/nginx/html/

# Copy the local HTML file to the NGINX default public directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 to allow external access
EXPOSE 80

# No need for CMD as NGINX image comes with a default CMD to start the server
```
![](5.%20vim.png)

`
echo "Welcome to Darey.io" >> index.html
`
![](6.%20echo.png)

**To build an image from this Dockerfile, navigate to the directory containing the file and run:**
```bash
docker build -t my-html-site .
```
![](7.%20docker%20build.png)

![](8.%20docker%20build.png)

![](9.%20docker%20images.png)

**To run a container based on the custom NGINX image we created with a dockerfile, run the command:**
```bash
docker run -d -p 8080:80 my-html-site
```
![](10.%20Create.png)

Running the command above will create a container that listens on port 8080 using the nginx image you created earlier. So you need to create a new rule in security group of the EC2 instance.

Let's recall the hands-on project we did in our advanced techops curriculum. Now let's add new rules to the security group

1. On our EC2 instance, click on the security tab:
![](11.%20Security.png)

2. Click on the inbound rule. to add the new rules. This will allow incoming traffic to instance associated with the security group. Our aim is to allow incoming traffic on port 8080.

![](12.%20Edit%20inbound%20rule.png)

3. Click on `add rule` to add a new rule

![](13.%20Add%20rule.png)

![](14.%20Save.png)

**List all available containers**
```bash
docker ps -a
```
![](15.%20docker.png)

**The image show our container is not yet running. We can start it with the command below:**
```bash
docker start CONTAINER_ID
```
![](16.%20docker%20start.png)

Now that we have started our container, we can access the content on our web browser with http://52.205.40.175:8080/

![](17.%20Page%20Accessed.png)

**Pushing Docker Images To Docker Hub**

Let's recall our git project, where we push changes made on our local computer to a remote repository (github) so everyone can track the chages we made and also collaborate on it. Now that we have created a docker images on our own computer, we need to think about how to reuse this image in the future or how do people in other region make use of this image and possibly collaborate on it. This is where Docker Hub comes in. Let's go ahead and push our image to <u>docker hub</u>

i. Create an account on <u>[Docker Hub](https://hub.docker.com/repositories/kaydollar01)</u> if you don't have one.

ii. Create a repository on docker hub

iii. Tag Your Docker Image Before pushing, ensure that your Docker image is appropriately tagged. You typically tag your image with your Docker Hub username and the repository name.

![](18.%20create%20repo.png)

```bash
docker tag <your-image-name> <your-dockerhub-username>/<your-repository-name>:<tag>
```
![](19.%20pushing.png)

iv. login to `docker hub`

![](20.%20Logged%20in.png)

v. Push your image to `docker hub`.
```bash
docker push <your-dockerhub-username>/<your-repository-name>:<tag>
```
![](21.%20Image%20Pushed.png)

vi. Verify the images in your docker hub repository.

![](22.%20verify%20push.png)









