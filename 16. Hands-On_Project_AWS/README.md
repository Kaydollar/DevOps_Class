# Introduction to Cloud Computing - Security and Identity Management(IAM)

**In this project,**

 we will be working with a hypothetical fintech startup named **Zappy e-Bank.** This fictitious company represents a typical startup venturing into the financial technology sector, aiming to leverage the cloud's power to innovate, scale, and deliver financial services. The scenario is set up to provide a realistic backdrop that will help you understand the application of **AWS IAM** in managing cloud resources securely and efficiently.


**The Importance of IAM for Zappy e-Bank**

For Zappy e-Bank, like any company dealing with financial services, security and compliance are paramount. The company must ensure that its data, including sensitive customer information, is securely managed and that access to resources is tightly controlled. AWS IAM plays a critical role in achieving these security objectives by allowing the company to define who is authenticated **(signed in)** and authorized **(has permissions)** to use resources.

**IAM will enable Zappy e-Bank to:**

* Create and manage AWS users and groups, to control access to AWS services and resources securely.

* Use IAM roles and policies to set more granular permissions for AWS services and external users or services that need to access Zappy e-Bank' AWS resources.

* Implement strong access controls, including multi-factor authentication (MFA), to enhance security.

**This project will walk you through setting up IAM for Zappy e-Bank, creating a secure environment that reflects real-world usage and challenges. Through this hands-on project, you will learn the fundamental of IAM, how to manage access to AWS resources, and best practices to secure your cloud environment.**

* **Project Goals and Learning Outcomes**

    **1. Gained a solid understanding of AWS IAM, including users, groups, roles, and policies.**

    **2. Learned how to apply IAM concepts to secure a fintech startup's cloud infrastructure.**

    **3. Developed practical skills in using the AWS Management Console to manage IAM.**

    **4. Understood the significance of secure access management and its impact on compliance and data security in the fintech industry.**

## Project Setup

* **Log-in to the AWS Console Management**

![My_Dashboard](1.%20Dashboard.png)

* **Navigate to the IAM Dashboard**

![IAM_Dashboard](2.%20IAM%20Dashboard.png)

### Create Policy for Development Team
* **Create Policy**

![Create_Policy](3.%20Create%20Policy.png)

* **In the select a service section, search for ec2.**

![EC2_select](4.%20EC2%20select.png)

* **"Checkbox" the `All EC2 actions`**

![All_EC2_actions](5.%20All%20EC2%20actions.png)

* **select `ALL` in the resources section**

![All](6.%20All.png)

Click Next

**Developers_Team Created**

![Developers_Team](7.%20Developers_team.png)

### Create Policy for the Data Analyst Team

* **Create Data_Analyst Policy**

![Analyst_Policy](8.%20Analyst%20Policy.png)

### Create Group for the Development team

1. In the IAM console, select user group. 

![](9.%20Group%20create.png)

2. Click `Create group` at the Top right corner of the screen. 

![Create_Group](10.%20Create%20Group.png)

3. Create the Developers-Team Group and Attach the Developer_Team Policy created earlier and create the Group.

![](11.%20Group%20for%20the%20Developers-Team.png)

4. Create the Analyst-Team Group and Attach the Developer_Team Policy created earlier and create the Group.

![](12.%20Group%20for%20the%20Analyst-Team.png)

* Groups Created.

5. You have successfully created a group and attached a permission policy for any user added to the group to have access to the EC2 instance only. Recall that users in this group will be backend developers only.

![](13.%20Analyst-Team%20Group%20and%20Developers-Team%20Group%20Created.png)

## Creating IAM User for John

Let's recall that John is a backend developer; therefore, he needs to be added as a user to the Development-Team group.

* Navigate to the IAM dashboard, select "Users" and then click "Create user".

![](14.%20Create%20user.png)

* Provide the name of the user. In this case, "John"

![](15.%20John%20Creating.png)

* Permissions: Add John to the development team group.

![](16.%20Permission%20for%20John.png)

* Click on Create User

![](17.%20Create%20user.png)

* Download the login credentials for John

![](18.%20Download%20Login.png)

## Creating IAM User for Mary.

![](19.%20Create%20user%20mary.png)

![](20.%20Mary%20Creating.png)

![](21.%20Create%20user.png)

![](22.%20Download%20Login.png)

### Testing and Validation

Login as John: Use the credentials provided for John to login into AWS Management Console. This simulates John User's experience and ensure he has the correct access.

![](23.%20Testing%20John%20User.png)

![](24.%20Password%20Change.png)

![](25.%20Continue%20to%20sign%20in.png)

![](26.%20John%20Logged%20in.png)

* **Access EC2 Dashboard**

![](27.%20John%20EC2%20Dashboard.png)

* **Perform EC2 Action**

![](28.%20John%20instance.png)

Login as Mary: Use the credentials provided for Mary to login into AWS Management Console. This simulates MAry User's experience and ensure he has the correct access.

![](29.%20Mary%20Logged%20in.png)

* **Access S3 Dashboard**

![](30.%20Mary%20Creating%20S3%20Bucket.png)

* **Perform S3 Action**

![](31.%20Bucket%20Created.png)

* **Validating Group policies**

![](32.%20Unathourised%20access.png)

![](33.%20Permission%20required.png)

### Setting Up MFA for John
**Multi-factor authentication (MFA):** is a security system that requires users to provide multiple forms of identification to access an account or system, adding an extra layer of security beyond just a password.

1. Click on User and then click on John. It is assumed we have already created a user account for John.

![](34.%20MFA%20John.png)

2. Click on enable MFA as shown in the image below.

![](35.%20Enable%20MFA.png)

3. Enter a device name for John MFA and select the authenticator app.

![](36.%20Authentication%20APP.png)

4. Click on Next
5. Open your Google Authenticator or Microsoft Authenticator application on your mobile device to scan the QR Code, then you can fill in the 2 consecutive codes as shown in the image below.

![](37.%20MFA.png)

6. By completing steps 1-5, MFA will be enabled for John.

![](38.%20Enabled%20MFA.png)

### Setting Up MFA for Mary.

![](39.%20Mary%20Enabled%20MFA.png)

### Project Reflection

In AWS, IAM roles are used to grant permissions to entities, allowing them to access AWS resources. They act as a security token, providing temporary credentials to users, applications, or services that need to interact with AWS services. This eliminates the need for long-term credentials and enhances security by limiting access to specific resources and operations. IAM roles are essential for managing access in various scenarios, including cross-account access, service-to-service communication, and access from outside AWS. 

Here's a more detailed look at the roles of IAM in AWS:

1. Granting Permissions: 
IAM roles are designed to define what actions a user, application, or service can perform within AWS.
They allow you to specify which AWS resources can be accessed and the level of access (e.g., read-only, read-write).
By using roles, you can avoid embedding long-term credentials directly into applications, enhancing security.

2. Managing Temporary Credentials: 
IAM roles provide temporary security credentials to entities that assume the role.
This means that instead of using long-term access keys, the entity receives a short-lived token that grants access for a limited time.
This practice reduces the risk of compromised credentials as the temporary tokens automatically expire.

3. Facilitating Cross-Account Access: 
IAM roles are crucial for allowing users or services in one AWS account to access resources in another account.
You can define a role in the resource account and configure it to be assumed by an entity in a different account.
This enables secure resource sharing and collaboration between different AWS accounts.

4. Enabling Service-to-Service Communication:
IAM roles are essential for allowing AWS services to interact with each other securely. 
For example, an EC2 instance can assume a role to access data stored in an S3 bucket or invoke a Lambda function. 
This allows services to perform their designated tasks without requiring explicit user credentials. 

5. Supporting External Workloads with IAM Roles Anywhere:
IAM Roles Anywhere extends the concept of IAM roles to workloads running outside of AWS. 
It allows servers, containers, and applications to obtain temporary AWS credentials for IAM roles and policies using X.509 certificates. 
This enables secure access to AWS resources from on-premises environments or other cloud providers. 

6. Centralized Access Management: 
IAM roles provide a centralized way to manage permissions and access control within AWS.

By defining roles and their associated policies, you can easily manage access for various users, applications, and services.

This simplifies security administration and ensures consistent access control across your AWS environment.

Identity and Access Management (IAM) in Amazon Web Services ...

In essence, IAM roles are a fundamental part of AWS security, enabling secure access to resources, managing temporary credentials, and facilitating interactions between different AWS services and even external workloads. 



