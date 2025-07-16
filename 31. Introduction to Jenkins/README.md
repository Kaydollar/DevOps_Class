# Introduction to Jenkins

## Introduction to CICD

Continuous Integration and Continuous Delivery (CI/CD) is a set of best practices and methodologies that revolutionize the software development lifecycle by enhancing efficiency, reloility, and speed.
CI/CD represents a seamless integration of automation and collaboration throughout the development| process, aiming to deliver high-quality software consistently and rapidly. In the realm of Cl, developers regularly integrate their code changes into a shared repository, triggering automated builds and tests to detect integration issues early. On the other hand, CD encompasses both Continuous Delivery and Continuous Deployment, ensuring that software is always in a deployable state and automating the deployment process for swift and reliable releases. The CI/CD pipeline fosters a culture of continuous improvement, allowing development teams to iterate quickly, reduce manual interventions, and deliver software with confidence.

## What is Jenkins

Jenkins is widely employed as a crucial CI/ CD tool for automating software development processes.
Teams utilize Jenkins to automate building, testing, and deploying applications, she amlining the development lifecycle. With Jenkins pipelines, developers can define, version, and execute entire workflows as code, ensuring consistent and reproducible builds. Integration with version control systems allows Jenkins to trigger builds automatically upon code changes, facilitating early detection of issues and enabling teams to deliver high-quality software at a faster pace. Jenkins' flexibility, extensibility through plugins, and support for various tools make it a preferred choice for organizations aiming to implement efficient and automated DevOps practices.

**Getting Started with Jenkins**

Update Package repositories
```bash
sudo apt update
```
![](1.%20sudo%20apt%20update.png)

**Install JDK**
```bash
sudo apt install default-jdk-headless
```
![](2.%20Install%20JDK.png)

**Install Jenkins**
```bash
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
    sudo apt update
    sudo apt-get install jenkins
```

![](3.%20Install%20Jenkins.png)

The command Install Jenkins, It involves importing the Jenkins GPG keyfor package verification, adding the Jenkins repository to the system's source, Updating Package lists, and finally installing Jenkins through the Package manager(apt-get)

**Check if Jenkins has been installed, and it is Up and Running.
```bash
sudo systemctl status jenkins
```
![](4.%20Jenkins%20status.png)

**Setup Jenkins on the web console**
i. Input your Jenkins instance ip address on your web browser i.e: http://public_ip_address:8080 

ii. on your Jenkins instance check "/var/lib/jenkins/secret/initialAdminPassword" to know your password.

![](5.%20web%20through%20ipaddress.png)

![](6.%20Password%20Fixed.png)

iii. Install suggested Plug-ins

![](7.%20Install%20suggested%20Plugins.png)

iv. Create a user account

![](8.%20Account%20Created.png)

![](9.%20Setup%20completed.png)

**Jenkins Job**

![](10.%20creating%20a%20JOB.png)

![](11.%20creating%20Job.png)

1. Go to the Jenkins Dashboard
In your browser:

http://your-ec2-ip:8080

Click on “New Item”
Enter a name (darey.io)

Select Freestyle project

Click OK

Configure the Project

You’ll land on the project configuration page.

Basic Setup:

Description (optional): "This job prints Hello World"

Under “Build” section:

- Click “Add build step” → choose “Execute shell”

- In the command box, enter:
```bash
echo "Hello from Jenkins!"
```
4. Save the Job
- Scroll down and click 

Run the Job
- Click “Build Now” (on the left sidebar)

![](13.%20Build%20Now.png)

6. Check the Output
- Under Build History, click the build number (#1)

- Click Console Output

![](14.%20Click.png)

![](15.%20Output.png)

### Next Step: Set Up Jenkins with GitHub

**Task:**

- Connect Jenkins to a GitHub repository

- Pull code from GitHub

- Run a build automatically or manually

1. Install Git Plugin (if not already)

- Go to:

```
Jenkins Dashboard → Manage Jenkins → Plugins → Available → Search: Git Plugin
```

2. Create a New Job

- Go to Jenkins dashboard → Click “New Item”

- Name: GitHub-Build-Job

- Type: Freestyle Project

- Click OK

3. Configure Git Repo

- Under “Source Code Management”:

- Select Git

- In Repository URL, paste your GitHub repo:

```
https://github.com/<your-username>/<your-repo>.git
```

If it’s private, you’ll need to add GitHub credentials.

![](16.%20source%20code.png)

4. Add GitHub Credentials (Optional if private repo)

- Go to Jenkins → Manage Jenkins → Credentials → Global → Add Credentials

- Choose Username and Password or use Personal Access Token if using GitHub's new token auth.

- Go back to the job → under Credentials, select the one you added.

5. Add a Simple Build Step
Under Build, click Add build step → Execute shell

- Example:

```
echo "Building project from GitHub"
ls -la
```
![](17.%20Build%20Step.png)

6. Save and Build
- Click Save

- Click Build Now

- View Console Output to see Jenkins pulling code from GitHub and executing your script

![](18.%20Output.png)

### GitHub Push → Jenkins Build Trigger

1. Enable GitHub Webhook Trigger in Jenkins Job
- Go to your Jenkins job (GitHub-Build-Job)

- Click Configure

- Scroll to Build Triggers

✅ Check “GitHub hook trigger for GITScm polling”

- Click Save

2. Expose Jenkins to the Internet
GitHub needs to reach Jenkins, so Jenkins must be publicly accessible (not just on localhost or private IP).

Options:
- Use a public EC2 IP/DNS + open port 8080 in the inbound rules.

- OR use ngrok to create a secure public tunnel:

```bash
./ngrok http 8080
```
3. Add a GitHub Webhook

- Go to your repo: github.com/Kaydollar/github-build-job

- Click on Settings > Webhooks > Add webhook

Set:

Payload URL:

perl
Copy
Edit
http://<your_public_jenkins_url>/github-webhook/

(Use https://abc123.ngrok.io/github-webhook/ if using ngrok)

Content type: application/json

✅ Just select "Push events"

- Click Add webhook

4. Ensure Git Plugin is Installed in Jenkins
- Go to Manage Jenkins > Manage Plugins

- Make sure Git Plugin and GitHub Integration Plugin are installed and updated

5. Test It

- Make a small commit to your repo

- Push it to GitHub

- Jenkins should trigger the build automatically 

### Creating and Managing Job in Jenkins

 1. Access Jenkins Dashboard
Open Jenkins in your browser:
http://localhost:8080 or your EC2/ngrok URL

![](32.%20EC2Ngok.png)

Log in using your credentials.

![](33.%20Logged-in.png)

2. Create a New Pipeline Job in Jenkins
Go to Jenkins Dashboard

Click "New Item"

![](34.%20New%20Item.png)

Enter a name like: pipeline-github-job

Select Pipeline

Click OK

![](35.%20Naming.png)

 2. Connect to GitHub Repository
In Pipeline Configuration:

Scroll to Pipeline section

Set:

Definition: Pipeline script from SCM

SCM: Git

Repository URL:

arduino
Copy
Edit
```
https://github.com/YOUR_USERNAME/YOUR_REPO.git
```
![](42.%20Github%20code.png)

![](36.%20Step%202.png)

Branch: */main or */master

Script Path: Jenkinsfile (default)

You can also add credentials if it’s a private repo.

![](37.%20Step%202.png)

3. Create a Jenkinsfile in Your Repo

In your local repo or on GitHub, create a file named:

```
Jenkinsfile
```
![](38,%20Step%203.png)

5. Trigger the Build in Jenkins
Go to Jenkins

Open the pipeline job

Click Build Now

![](39.%20Build%20Now.png)

You can view progress in Blue Ocean plugin or Console Output

![](40.%20Build.png)

![](41.%20Success.png)
