# Mini Project: Configuring Auto Scaling with ALB using Launch Template

In this mini project, you will learn how to configure Auto Scaling in AWS with an Application Load Balancer (ALB) using a Launch Template. The project aims to demonstrate the automatic scaling of EC2 instances based on demand, while leveraging the benefits of a Launch Template.

## Objectives:

### Create Launch Template:

* Learn how to create a Lauch Template with the required specifications

### Set Up Auto Scaling Group:

* Configure an Auto Scaling group to manage the desired number of EC2 instances using the Launch Template.

### Configure Scaling Policies:

* Set up scaling policies to adjust the number of instances based on demand.

### Attach ALB to Autoscaling Group:

* Connect the Auto Scaling group to an existing ALB

### Test Auto Scaling:

* Verify that the Auto Scaling group adjusts the number of instances in response to changes in demand

### Create Launch Template

* Log in to the AWS Management Console

![](1.%20Log-in.png)

* Navigate to the EC2 service
* In the left panel, clcik on Lunch Template
* Click create template

![](2.%20Launch%20Template.png)

* Configure the Launch Template setting, include the AMI, Instance type, and user data

![](3.%20naming.png)

* LT Created successfully

![](4.%20Successful.png)

### Setup Auto Scaling Group

* In the AWS console, navigate to the EC2

* Click on Auto Scaling Group (ASG)

![](5.%20Auto-Scaling.png)

* provide the ASG informations
* click next

![](6.%20naming.png)

* Choose and existing VPC and Subnets

![](7.%20VPC%20and%20Subnets.png)

* ASG


* click next

![](8.%20naming.png)

* skip to review

![](9.%20Skip.png)

* review for errors
* click `Create Autoscaling Group`

![](10.%20Create%20Auto%20Scaling%20Group.png)

* Successfully created ASG

![](11.%20Successful.png)

* Instance launched successfully

![](12.%20instances%20running.png)

### Configure Scaling Policy

* In the Auto scaling group configuration, navigate to the "scaling policy section"

![](13.%20policy.png)

* click on create scaling policy
* click on create

![](14.%20Policy%20tracking.png)

* Configure policy for scaling in and out
* successfully created scaling policy

![](15.%20Successful.png)

### Attach ALB to Auto Scaling Group

![](16.%20Load%20Balancer.png)

* Select the ALB to associate with the Auto Scaling Group

![](17.%20Load%20balancer%20create.png)

* choose a load balancer name

![](18.%20Configure.png)

* network mapping, choose your VPC
* select your AZ ad subnet

* target group
* choose a target type
* click on next

![](19.%20Target%20Group.png)

* Load balanacer created successfully

![](20.%20Successful.png)

### Test Auto Scaling

* Generate traffic to trigger scaling policy
* connect to EC2 instance and run this command.

```sudo
        sudo amazon-linux-extras install epel -y
       sudo yum install stress -y

       stress -c 4 
```

* Monitor the auto scaling group and verify that the number of instances adjust based on demand.

* paste dns of application load balance on browser.







