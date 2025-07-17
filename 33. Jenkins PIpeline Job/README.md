# Jenkins Pipeline Job

## What is Jenkins Pipeline Job

A Jenkins pipeline job is a way to define and automate a series of steps in the software delivery process. it allows you to script and organize your entire build, test, and deployment. Jenkins pipelines enable organizations to define, visualize, and execute intricate build, test, and deployment processes as code.
This facilitates the seamless integration of continuous integration and continuous delivery (CI/ CD) practices into software development.

Let's recall our docker foundations project when we created a dockerfile and made a docker image and container with it. Now let's automate the same process with jenkins pipeline code.

**Creating a Pipeline Job**

i. From the dashboard menu on the left side, click on new item. 

![](1.%20New%20Item.png)

ii. Create a pipeline Job and name it "My Pipeline Job"

![](2.%20Pipeline%20Job.png)

**Configuring Build Trigger**

i. Click Configure

![](3.%20Configure.png)

![](4.%20Configure.png)

![](5.%20Configure.png)

![](6.%20Configure.png)

![](7.%20Pipeline%20Script.png)

**Writing Jenkins Script**

A jenkins pipeline script refers to a script that defines and orchestrates the steps and stages of a continuous integration and continuous delivery (CI/CD) pipeline. Jenkins pipelines can be defined using either declarative or scripted syntax. Declarative syntax is a more structured and concise way to define pipelines. It uses a domain-specific language to describe the pipeline stages, steps, and other configurations while scripted syntax provides more flexibility and is suitable for complex scripting requirements.

**Fixed Jenkinsfile (Declarative Pipeline)**

```bash
pipeline {
    agent any

    stages {
        stage('Connect To GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Kaydollar/my-pipeline-job.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-html-image .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop old container if running
                    sh 'docker rm -f my-html-container || true'
                    // Run new container
                    sh 'docker run -d -p 8081:80 --name my-html-container my-html-image'
                }
            }
        }
    }
}
```

**Explanation of Script**

The provided Jenkins pipeline script defines a series of stages for a continuous integration and continuous delivery (CI/CD) process. Let's break down each stage:

- Agent Configuration

`
agent any
`

Specifies that the pipeline can run on any available agent (an agent can either be a jenkins master o node). This means the pipeline is not tied to a specific node type.

- Stages

```
stages {"\n      // Stages go here\n   "}
```

Defines various stages of the Pipeline, each representing a phase in the software delivery process.

- Stage 1: Connect to Github
```
stage('Connect To Github') {"\n      steps {\n         checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/RidwanAz/jenkins-scm.git']])\n      "}
}
```

This stage checks out the source code from a Github Repository

(https://github.com/Kaydollar/my-pipeline-job.git)

It specifies that the pipeline should use the 'main' branch 


- **Stage 2: Build Docker Image**

```
stage('Build Docker Image') {"\n      steps {\n         script {\n            sh 'docker build -t dockerfile .'\n         "}
   }
}
```

This stage build Docker-Image named 'dockerfile' using the source code obtained from the Github Repository

The `docker build` command is executed using the shell (`sh`).

- **Stage 3: Run Docker Container**

```
stage('Run Docker Container') {"\n      steps {\n         script {\n            sh 'docker run -itd --name nginx -p 8081:80 dockerfile'\n         "}
   }
}
```

This stage runs a Docker container named 'nginx' in detached mode (-itd).

 The container is mapped to port 8081 on the host machine (* -p 8081:80*).

The Docker image used is the one built in the previous stage ('dockerfile").

**Copy the pipeline script and paste it in the Section below**

![](7.%20Pipeline%20Script.png)

i. Click on the pipeline script

![](8.%20Pipeline%20Syntax.png)

ii. Select the dropdown and search for `checkout: check out from version system`

![](9.%20Checkout.png)

iii. Paste your urland make sure the branch is main

![](10.%20Paste%20url.png)

iv. Generate pipeline script

![](11.%20Script.png)

```bash
checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Kaydollar/my-pipeline-job.git']])
```

Now you can replace the generated script with for connect jenkins with Github

### Installing Docker

Before Jenkin can run Docker commands, we need to install Docker on the same instance jenkins was installed. from our shell scripting knowledge, lets install docker with script

* Create a file called docker.sh
![](12.%20docker%20file.png)

ii. Open the file and paste the script below:
```bash
sudo apt-get update -y
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update -y
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo systemctl status docker
```
![](13.%20Paste%20and%20save.png)

iii. make the file Executable
```bash
chmod u+x docker.sh
```
iv. Execute the file
```bash
./docker.sh
```
![](14.%20Run%20docker%20script.png)

### Building Pipeline Script

Now that we have docker installed on the same instance with jenkins, we need to create a dockerfile before we can run our pipeline script. As we know, we cannot build a docker image without a dockerfile.
Let's recall the dockerfile we used to build a docker image in our docker foundations. In the main branch on `jenkins-scm`,

i. Create a new file called `dockerfile`
![](15.%20Create%20dockerfile.png)

ii. Paste the snippet below in the file.
```bash
# Use the official NGINX base image
FROM nginx:latest

# Set the working directory in the container
WORKDIR  /usr/share/nginx/html/

# Copy the local HTML file to the NGINX default public directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 to allow external access
EXPOSE 80
```
iii. create an index.html and paste the content below.  
```bash
Congratulations, You have successfully run your first pipeline code.
```
Pushing the `dockerfile` and `index.html` file will trigger the Jenkins to automatically run new build for our pipeline. 

![](16.%20Pushed.png)

To access the content of `index.html` on the web browser we must edit the first inbound rule and open the port we mapped the container to (8081)

![](17.%20Edit%20inbound%20rule.png)

We can now access the content of index.html on the web browser
```
http://jenkins-ip-address:8081
```

