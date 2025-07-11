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

