# AWS Identity and Access MAnagement Mini Project.

**Project Goals**
* Understand AWS Identity and Access Management(IAM) Principles and Components
* Learn to create and manage IAM policies for regulating access to AWS resources securely.
* Apply IAM concepts pratically to control access within the AWS environments
* Explore best practices for IAM implementations and security in AWS.

**Learning Outcomes**
* Recognize IAM components like users, roles, policies, and groups.
* Creates and manage IAM polices to define permissions to users and roles.
* Setup IAM users, groups and roles to control access to AWS services.
* Understands IAM best practices for maintaining securites, and managing access to AWS resources.

**What is IAM:**  
* AM, or Identity and Access Management. Think of it as the gatekeeper for your AWS a resources, its job is to decide who gets in and what they're allowed to do once they're inside.
Imagine you have this big digital "house" full of all your AWS stuff-your data, your applications, the whole shebang. Now, you don't want just anyone wandering in and messing around with your things, right? That's where IAM steps in.
It's like having your own VIP list for your digital world. IAM helps you keep your AWS resources safe and sound, making sure only the right people get in and that they're only allowed to do what you say they can. It's all about keeping your digital house in order and protecting your precious stuff from any unwanted guests.

Note- AWS resources are the various services and tools provided by Amazon Web Services (AWS) that users can utilize to build and manage their applications and infrastructe in the Cloud

**IAM User:**
* IAM users are like individual accounts for different people or entities within your AWS environment.
For example, if you have a team working on a project, you can create separate IAM users for each team member. Each IAM user would have their own unique username and password, allowing them to access the AWS resources they need for their work.
IAM users help you manage and control access to your AWS resources securely, ensuring that each user only has access to the resources they need to perform their tasks.

**IAM Role**
* An IAM role defines what someone or something (like an application or service) can do within your AWS account. Each role has a set of permissions that determine which actions it can perform and which AWS resources it can access.
For example, you might have an "admin" role that gives full access to all resources, or a "developer" role that only allows access to certain services for building applications.
Or if we take another example, imagine you have a visitor who needs temporary access to your house to fix something. Instead of giving them a permanent key (IAM user), you give them a temporary key (IAM role) that only works for a limited time and grants access to specific rooms (AWS resources).
IAM roles are flexible and can be assumed by users, services, or applications as needed. They are commonly used for tasks such as granting permissions to AWS services, allowing cross account access, or providing temporary access to external users. IAM roles enhances security and efficiency for providing controlled access to AWS resources without the need for permanent credentials. 

**IAM Policy:**
* An IAM policy is a set of rules that define what actions a role can take. These rules specify the permissions granted to the role. Think of a policy as a rulebook for the role. It outlines which actions are allowed and which are not, helping to ensure secure and controlled access to your AWS resources.
For example, the rulebook might say that the "admin" key (IAM role or user) can open any door and perform any action within the house (AWS resources), while the
"viewer" key (IAM role or user) can only open certain doors and look around, but not make any changes.
IAM policies define the permissions granted to IAM roles and users, specifying which AWS resources they can access and what actions they can take. They are essential for maintaining security and controlling access to AWS resources, ensuring that only authorized actions are performed by users and roles within your AWS account.

**IAM group**
* IAM Groups are like collections of IAM users. Instead of managing permissions for each user individually, you can organize users into groups based on their roles or responsibilities.
You can think IAM Groups as these neat little collections of users with similar roles or responsibilities. It's like putting everyone into teams based on their tasks. So, you might have a group for developers, another for administrators, and so on. So instead of setting permissions for each person one by one, you set them up for the whole group at once.
For example, let's say you have a development team working on a project. Instead of assigning permissions to each developer one by one, you can create an IAM Group called "Developers" and add all the developers to that group. Then, you assign permissions to the group as a whole. So, if you want all developers to have acrace to the same resources, you only need to set it up once for the group. 

**Best Practices**
* Give only the permissions needed: Don't give more access than necessary.
* Use roles instead of users: Roles are safer and can be used when needed.
* Review roles regularly: Remove unused roles to keep things tidy and secure.
* Add extra security with MFA: Use Multi-Factor Authentication for extra protection.
* Use ready-made policies: They're safer and easier to use
* Keep policies simple: Make separate policies for different tasks.
* Keep track of changes: Keep a record of who changes what.
* Test policies before using them: Make sure they work the way you want them to before applying them to real stuff.
* Use descriptive names: Choose clear and descriptive names for lAM groups to facilitate understanding and management.
* Enforce strong password policies: Encourage users to create strong passwords and implement expiration and complexity requirements.

**Note- (difference between users and roles) In AWS, users are like individual people with their own set of keys to access resources. These keys are permanent and tied to specific individuals. It's similar to having your own key to the front door of your house
-it's always yours.**

**On the other hand, roles in AWS are more like special keys that can be used by different people or even programs. These keys provide temporary access and can to be used by different users as needed. Roles are like master keys that can be used by anyone who needs access to certain things temporarily. So, while users are tied to specific individuals, roles are more flexible and can be shared among different users for specific task**

## PART 1

1. Navigate to the AWS Management Console.

a. Use the search bar to locate the Identity and Access Management(IAM) services.

![](1.%20search%20IAM.png)

2. Now, on the IAM dashboard, navigate to the left sidebar and click on "policies."

a. From there, search for EC2 and select "AmazonEC2FullAccess" from the list of Policies.
b. Proceed by clicking create Policy to initiate the policy creation process. 

![](2.%20Search%20EC2.png)

3. Select all EC2 actions.

![](3.%20All%20EC2%20actions.png)

4. Tick "All Resources" and Click "Next" 

![](4.%20All%20Resources.png)

5. Click on "Create Policy"

![](5.%20Create%20Policy.png)

This is the Policy we have created.

![testpolicy](6.%20testpolict%20created.png)

6. Proceed to the "Users" and proceed to create a new user by clicking on "create user"

![](7.%20Create%20User.png)

a. Then select the option "Provide user access to AWS management Console" if access to the web-based console interface is required.

b. proceed to setup a password for the user.

c. check the box "Users must create a new password at next sign-in" if allowing users to change there password upon first sign-in is preffered. 

8. Select "Attach Policy Directly" and navigate to "Filter customer managed policy" 

a. Choose the policy you created named "testpolicy"
b. The Proceed by clicking Next

![](9.%20Attach%20Policy.png)

**Notes**
* Managed Policies: Made by AWS, Used by many.
* Customed Managed Policies: You make and Managed them.
* Inline Policies: Made for one specific thing. 

9. Click on "create user"

![](10.%20User%20Created.png)

10. Ensure to save this details securely fpr future references.

a. Click on "Return to User List"

![](11.%20SAve.png)

Eric User has been successfully created, and the policy granting him full Access to EC2 has been attached.

**Part 2**

1. On the User Groups Section, Enter a name for the Group. 

a. Click on Create "User Group".

b. Then, proceed to the "Users" section.

c. Proceed to click "Create User group"

![](12.%20Group%20Create.png)

 Proceed to "Users" Section

![](13.%20Developers-Team.png)

2.  Now Lets create a user name "Jack"

![](14.%20Jack%20User%20create.png)

3. In the "Permissions" options, select "Add user to the group"

a. Then, In the "User groups" section,

b. Choose the group you created named "Developers-Team" 

c. Click "Next".

![](15.%20Jack%20Permission.png)

4. Click on "create user"

![](16.%20Create%20user.png)

Repeat Same Process for Ade. create User Ade and Add him to the group "Developers-Team". 

![](Ade%20User%20Create.png)

5. Navigate to the "Policies" section and click on "Create policy" to begin crafting a new policy.

![](18.%20Policy%20Create.png.)

6. Choose the two services, EC2 ans S3, from the available options.

![](19.%20EC2%20and%20S3%20Policy.png)

7. Enter the desired Policy Name `testpolicy2` and proceed to click on the "Create Policy" button.

![](20.%20Review%20and%20Create%20Policy.png)

8. Navigate to the "User groups" and select the "Developers-Team" group. 

![](21.%20Developers-Team(User%20groups).png)

9. Proceed to the "Permissions" section and add the neccessary permissions. 

![](22.%20Add%20Permissions.png)

10. Click on Attach policy.

![](23.%20Attach%20Policies.png)

11. Select "Customer Managed Policy" as the Policy type.

a. Then choose the "Developers-policy you created.

b. Click "Attach policy"

![](24.%20customer%20Attached%20Policy.png)

The Policy is now attached to the group, granting FULL Access to EC2 and S3 for the group users.

![](25.%20Attached%20Policy%20to%20group.png)

**Project Reflection**

* Understanding IAM: IAM serves as the security foundation for AWS resources, controlling access and permissions.

* Security Importance: IAM ensures data protection, compliance, and prevents unauthorized access.

* Policy Creation: Participants learned to craft IAM policies to regulate resource access effectively.

* Practical Application: Hands-on exercises equipped participants to set up lAM users, groups, and roles, enhancing their real-world IAM implementation skills.




