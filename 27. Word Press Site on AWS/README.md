# CAPSTONE PROJECT - WordPress Site on AWS

## Project Scenario
A small to medium-sized digital marketing agency, "DigitalBoost", wants to enhance its online presence by creating a high-performance WordPress-based website for their clients. The agency needs a scalable, secure, and cost-effective solution that can handle increasing traffic and seamlessly integrate with their existing infrastructure. Your task as an AWS Solutions Architect is to design and implement a WordPress solution using various AWS services, such as Networking, Compute, Object Storage, and Databases.

### Project component.

1. VPC Setup

 * 1.1 Create VPC
    
    VPC CIDR: 10.0.0.0/16

    Name: `digitalboost-vpc`

![](1.%20Create%20VPC.png)

![](2.%20Creating%20VPC.png)

* VPC Created Successfully. 

![](3.%20Successful.png)

* 1.2 Create Subnets

![](4.%20Create%20Subnet.png)

| Subnet Type | AZ         | CIDR        |
| ----------- | ---------- | ----------- |
| Public 1    | us-east-1a | 10.0.1.0/24 |
| Public 2    | us-east-1b | 10.0.2.0/24 |
| Private 1   | us-east-1a | 10.0.3.0/24 |
| Private 2   | us-east-1b | 10.0.4.0/24 |

![](5.%20Creating%20Subnets.png)

![](6.%20Successful.png)

* 1.3 Create Internet Gateway

![](7.%20Create%20IGs.png)

![](8.%20Create%20IGs.png)

Attach to digitalboost-vpc

![](9.%20Attach%20VPC.png)

![](10.%20Attach%20VPC.png)

🔹 1.4 Create Route Tables

![](11.%20Create%20RT.png)

![](12.%20Creating%20RT.png)

Public route table → add route to IGW (0.0.0.0/0)

![](13.%20Edit%20route.png)

Associate it with public subnets

![](14.%20Route%20Ass.png)

2. Launch RDS (MySQL) in Private Subnet

* Go to RDS > Create Database

![](15.%20Create%20SG.png)

![](16.%20Create%20DB.png)

* Engine: MySQL

* Use Multi-AZ

* DB instance class: db.t3.micro (for test) or db.t3.medium

* Storage: 20 GB (enable auto-scaling)

* Credentials: admin / yourpassword

* VPC: digitalboost-vpc

* Subnet group: private subnets

* Security group: allow port 3306 from EC2 subnet CIDRs

![](18.%20DB.png)

![](19.%20Successful.png)

### Step 3: Create S3 Bucket (Media Storage)
* Bucket name:  

* digitalboost-wordpress-media

* Enable public access (optional) or use signed URLs

* Enable versioning and encryption

* Use IAM Role to allow EC2 access to S3

![](20.%20S3%20bucket%20created.png)

### Step 4: Launch EC2 with WordPress
* 4.1 Create Security Group for EC2

* Allow:

* Port 22 (SSH) from your IP

* Port 80, 443 from ALB

![](22.%20SG%20create%20success.png)

To create a security group for an EC2 instance allowing SSH access from your IP and web traffic from an ALB, you need to configure inbound rules for the security group.
 
Steps to create the security group:

Access the EC2 Console: Open the Amazon EC2 console. 

Navigate to Security Groups: In the navigation pane, select "Security Groups". 

Create a New Security Group: Click "Create Security Group". 

Basic Details:

Provide a Security group name (e.g., my-ec2-sg).

Optionally, add a Description. 
Select the VPC where your EC2 instance will reside. 
Inbound Rules:

For SSH (Port 22):

Click "Add rule". 

Type: Select "SSH" or "Custom TCP". 
Protocol: TCP is selected by default for SSH. 

Port Range: Enter 22. 

Source: Select "My IP" or enter your public IP address in CIDR notation (e.g., YOUR_IP_ADDRESS/32). Using your specific IP is more secure than "Anywhere". 

Description (optional): "Allow SSH from my IP". 

For ALB Traffic (Ports 80 and 443):

Click "Add rule". 
Type: Select "HTTP" for port 80 and "HTTPS" for port 443, or "Custom TCP" for both. 

Protocol: TCP is selected by default for HTTP/HTTPS. 

Port Range: Enter 80 for HTTP and 443 for HTTPS. 

Source: This is crucial for ALB integration. You'll typically want to allow traffic from the security group associated with your Application Load Balancer (ALB). Find your ALB's security group and use its ID as the source for these rules, rather than "Anywhere" or specific IPs. 

Description (optional): "Allow HTTP 
from ALB" and "Allow HTTPS from ALB". 

Create Security Group: Click "Create Security Group" to finalize. 
Important Considerations:

Your Public IP:

You can find your public IP by searching "what is my ip" in a web browser. 

ALB Security Group:

When creating your Application Load Balancer, it will also have an associated security group. The EC2 security group should allow traffic from the ALB's security group on ports 80 and 443. 

Security Group Propagation:
Changes to security groups are propagated to instances associated with the group, but there might be a small delay. 

Stateful Nature:

Security groups are stateful, meaning if you allow incoming traffic, the return traffic is automatically allowed without needing outbound rules. 

* 4.2 Launch EC2 (Amazon Linux 2 or Ubuntu)

* Use a public subnet

* Key pair: create or use existing

* Attach IAM role with:

* AmazonS3FullAccess

* CloudWatchAgentServerPolicy

![](23.%20EC2%20instance%20created.png)

![](24.%20LB%20create.png)

![](25.%20ALB%20created.png)

### Step 5: Create ALB (Application Load Balancer)
* Type: Internet-facing

* Listener: HTTP (port 80)

* Target Group: Register your EC2

* Health Check: /index.php

Attach to public subnets

* Security group: Allow HTTP/HTTPS from anywhere

![](26.%20ALB%20created.png)


### Step 6: Enable Auto Scaling Group
Create Launch Template using EC2 config

![](27.%20Launch%20Template.png)

Create Auto Scaling Group:

Attach to ALB

Min 2 / Max 4 instances

Use Target Tracking Policy on CPU (e.g., 50%)

![](28.%20AS%20Create.png)

![](29.%20Successful.png)

### Step 7: Domain Setup via Route 53

Register or migrate domain

Create A/ALIAS record pointing to ALB DNS

Use ACM to create an SSL cert

Add HTTPS listener to ALB

### Step 8: Add Security and Monitoring

