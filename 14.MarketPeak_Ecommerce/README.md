# Capstone Project: E-Commerce Platform Deployment with Git, Linux, and AWS

## 1. Implement Version Control with Git

### 1.1. Initialize Git Repository

* **Create a Project Directory**

  * Start by creating a new directory for your project named MarketPeak_Ecommerce:

```bash
mkdir MarketPeak_Ecommerce
cd MarketPeak_Ecommerce
```
![](1.%20Mkdir.png)

* **Initialize Git**

  * Inside the project directory, initialize a Git repository to enable version control:

```bash
  git init
```
![](2.%20git%20init(master).png)

This will set up a new, empty Git repository in the MarketPeak_Ecommerce directory.

### 1.2. Obtain and Prepare the E-Commerce Website Template

* Download a Website Template: Visit Tooplate or any other free template resource, and download a suitable e-commerce website template. Look for templates that are ready to use and require minimal adjustments.

* Prepare the Website Template: Extract the downloaded template into your project directory, MarketPeak_Ecommerce.

### 1.3. Stage and Commit the Template to Git

* Add your website files to the Git repository.

* Set your Git global configuration with your username and email.

* Commit your changes with a clear, descriptive message.

```bash
git add .
git config -- global user.name "YourUsername"
git config -- global user.email "youremail@example.com"
git commit -m "Initial commit with basic e-commerce site structure"
```

![](3.%20git%20add.png)
![](4.%20git%20config%20username.png)
![](5.%20git%20config%20email.png)
![](6.%20git%20commit.png)

### 1.4 Push the code to your Github repository

* After initializing your Git repository and adding your e-commerce website template, the next step is to push your code to a remote repository on GitHub. This step is crucial for version control and collaboration.

    * Create a Remote Repository on GitHub: Log into your GitHub account and create a new repository named "MarketPeak_Ecommerce" Leave the repository empty without initializing it with a README, .gitignore, or license.

    ![](7.%20Empty%20repo.png)

    * Link Your Local Repository to GitHub: In your terminal, within your project directory, add the remote repository URL to your local repository configuration.  `git remote add origin https://github.com/your-git-username/MarketPeak_Ecommerce.git`

![](8.%20git%20remote%20add%20origin.png)

## 2. AWS Deployment

To deploy "MarketPeak_Ecommerce" platform, you'll start by setting up an Amazon EC2 instance:

### 2.1. Set Up an AWS EC2 Instance
* Log in to the AWS Management Console.
* Launch an EC2 instance using an Amazon Linux AMI.
* Connect to the instance using SSH.

[Launch an EC2 instance using an Amazon Linux AMI](https://eu-north-1.console.aws.amazon.com/console/home?region=eu-north-1)

### 2.1 Clone the repository on the Linux Server
Before deploying your e-commerce platform, you need to clone the GitHub repository to your AWS EC2 instance. This process involves authenticating with GitHub and choosing between two primary methods of cloning a repository: SSH and HTTPS. To see the ssh or http link to clone your repository

![](9.%20ssh%20client.png)

* Navigate to your repository in github console
* Select the ` code' as highlighted in the image below.

![](10.%20ssh-keygen.png)

### 2.3. Install a Web Server on EC2
**Apache HTTP Server:** is a widely used web server that serves HTML files and content over the internet. Installing it on Linux EC2 server allows you to host MarketPeak E-commerce site:
* To install the Apache HTTP Server (httpd) on Ubuntu, follow these steps:
    * Update your package list:  `sudo apt update`

![](11.%20apt%20update.png)

* Install Apache2 on Ubuntu: `sudo apt install apache2 -y`

![](12.%20install%20apache.png)

* Start and enable the Apache service:

    `sudo systemctl start apache2`

    `sudo systemctl enable apache2`

![](13.%20systemctl%20enable.png)

* Check status to ensure it's running:  `sudo systemctl status apache2`

![](14.%20systemctl%20status.png)

### 2.4. Configure httpd for Website

To serve the website from the EC2 instance, configure httpd to point to the directory on the Linux server where the website code files are stored. Usually in /var/www/html.

* Prepare the Web Directory: Clear the default apache web directory and copy MarketPeak Ecommerce website files to it.

* Reload Apache: Apply the changes by reloading the apache service.
```bash
sudo rm -rf /var/www/html/*
sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/

sudo systemctl relaod apache2
```
![](15.%20Copy.png)

### 2.5. Access Website from Browser

* With httpd configured and website files in place, MarketPeak Ecommerce platform is now live on the internet:

* Open a web browser and access the public IP of your EC2 instance to view the deployed website.

## 3. Continuous Integration and Deployment Workflow

To ensure a smooth workflow for developing, testing, and deploying your e-commerce platform, follow this structured approach. It covers making changes in a development environment, utilizing version control with Git, and deploying updates to your production server on AWS.

### 3.1 Developing New Features and Fixes
* Create a Development Branch: Begin your development work by creating a separate branch. This isolates new features and bug fixes from the stable version of your website.
```bash
git branch development
git checkout development
```
![](16.%20git%20branch.png)

### 3.2 Version Control with Git
* Stage Your Changes: After making your changes, add them to the staging area in Git. This prepares the changes for a commit.

`git add .`

* Commit Your Changes: Securely save your changes in the Git repository with a commit. Include a descriptive message about the updates. 

`git commit -m "Add new features or fix bugs"`

* Push Changes to GitHub: Upload your development branch with the new changes to GitHub. This enables collaboration and version tracking.

`git push origin development`

![](17.%20Development.png)

### 3.3 Pull Requests and Merging to the Main branch

* Create a Pull Request (PR): On GitHub, create a pull request to merge the development branch into the main branch. This process is crucial for code review and maintaining code quality.

* Review and Merge the PR: Review the changes for any potential issues. Once satisfied, merge the pull request into the main branch, incorporating the new features or fixes into the production codebase.

![](19.%20Merge.png)

