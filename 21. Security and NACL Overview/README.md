# Security and NACL Overview Mini-Project.

This project focuses on Security Groups and Network Access Control Lists (NACLs). Our objective is to understand these fundamental componets of AWS infrastructure, including how Security Groups control inbound and outbound traffic to EC2 instances, and how NACLs act as subnet-level firewalls, regulating traffic entering and exiting subnets. Through practical demonstrations and interactive exercises, we'll navigate the AWS management console to deploy and manage these critical components effectively.

## Project Goals:

* Understand the concepts of Security Groups and Network Access Control Lists (NACLs) in AWS.

* Explore how Security Groups and NACLs function as virtual firewalls to control inbound and outbound traffic

* Gain hands-on experience with configuring security groups and NACLS to allow or deny specific types of traffic.

## Learning Outcome:

* Gain proficiency in configuring Security Groups and NACLs to control network traffic within AWS environments.

* Understand the differences between Security Groups and NACLs, including their scope, statefulness, and rule configurarions.

* Learn how to troubleshoot network connectinvity issues by analyzing security Group and NACLs configurations.

* Develop a deeper understanding of AWS network concepts and best practices for securing cloud environments.

## Security Group (SG):

**Inbound Rules:** Rules that control the incoming traffic to an AWS resource, such as an EC2 instance or an RDS database.

**Outbound Rules:** Rules that control that outgoing traffic from an AWS resource.

**Stateful:** Security groups automatically allow return traffic initiated by the instances to which they are attached.

**Port:** A communication endpoint that processes incoming and outgoing network traffic. Security groups use ports to specify the types of traffic allowed.

**Protocol:** The set of rules that governs the communication between different endpoints in a network. Common protocols include TCP, UDP, and ICMP.

## Network Access Control Lists (NACLs):

**Subnet-level Firewall:** NACLs act as a firewall at the subnet level, controlling traffic entering and exiting the subnet.

**Stateless:** Unlike security groups, NACLs are stateless, meaning they do not automatically allow return traffic. you must explicitly configure rules for both inbound and outbound traffic.

**Allow/Deny:** NACLs rules can either allow or deny traffic based on the specified criteria.

**Ingress:** Refers to inbound traffic, i.e., traffic entering the subnet.

**Egress:** Refers to outbound traffic, i.e., traffic exiting the subnet.

**CIDR Block:** Specifies a ranges of IP addresses in CIDR notation (e.g., 10.0.0.0/24) that the NACL rule applies to.

## Default Settings:

**Default Security Group:** Every VPC comes with a default security group that allows all outbound traffic and denies all inbound traffic by default.

**Default NACL:** Every subnet within a VPC is associated with a defaulty NACL that allows all inbound and outbound traffic by default.

## What is Security Group?

Imagine you're hosting a big party at your house. You want to make sure only the people you invite can come in, and you also want to control what they can do once they're inside.

AWS security groups are like bouncers at the door of your party. They decide who gets to come in (inbound traffic) and who gets kicked out (outbound traffic). Each security group is like a set of rules that tells the bouncers what's allowed and what's not.

For example, you can create a security group for your web server that only allows traffic on port 80 (the standard port for web traffic) from the internet. This means only web traffic can get through, keeping your server safe from other kinds of attacks.

Similarly, you can have another security group for your database server that only allows traffic from your web server. This way, your database is protected, and only your web server can access it, like a VIP area at your party.

In simple terms, security group act as barriesrs that control who can access your AWS resources and what they can do once the're in. They're like digital bouncers that keep your party (or your cloud) safe and secure.

## What is NACL?

NACL stands for Network Access Control List. Think of it like a security checkpoint for your entire neighbourhood in the AWS cloud. Imagine your AWS resources are houses in a neighbourhood, and you want to control who can come in and out. That's where NACLs come in handy.

NACLs are like neighbourhood security guards. They sit at the entrance and check every person (or packet of data) that wants to enter or leave the neighbourhood.

But here's the thing: NACLs work at the subnet level, not the individual resource level like security groups. So instead of controlling access for each house (or AWS resource), they control access for the entire neighbourhood (or subnet).

You can set rules in your NACL to allow or deny traffic based on things like IP addresses, protocols, and ports. For example, you can allow web traffic (HTTP) but block trafic on other ports like FTP or SSH

Unlike security groups, which are stateful (meaning the remember previous interations), NACLs are stateless. This means you have to explicitly allow inbound and outbound traffic separately, unlike security groups where allowing inbound traffic automatically allows outbound traffic related to that session.

In simple terms, NACLs act as gatekeepers for your AWS subnets, controlling who can come in and out based on a set of rules you define. They're like the security guards that keep your neighbourhood (or your AWS network) safe and secure.

## Difference between Security Groups and NACL

Security Groups in AWS act like virtual firewalls that control traffic at the instance level. They define rules for inbound and outbound traffic based on protocols, ports, and IP addresses. Essentially, they protect individual instances by filtering traffic, allowing only authorized communication.

On the other hand, Network Access Control Lists (NACLs) fucntion at the subnet level, overseeing traffic entering and leaving subnets. They operate as a barrier for entire subnets, filtering traffic based on IP addresses and protocol numbers. Unlike security groups, NACLs are stateless, meaning they don't remember the state of the connection, and each rule applies to both inbound and outbound traffic independently.

**Note-** In security groups, there's no explicit deny option as seen in NACLs; any rule configured within a security group implies permission, meaning that if a rule is established, it's automatically allowed.

Let's come to the practical part' The practical will be in two parts.

1. Security Group
2. NACL

## Hands-on Security Group

Initially we'll examine the configuration of inbound and outbound rules for security groups.

Create a security group allowing HTTP for all traffic and attach it to the instance.

## Explore various scenarios:

implement inbound traffic rules for HTTP and SSH protocols and allow outbound traffic for all.

Configure inbound rules for HTTP with no outbound rules.

Remove both inbound and outbound rules.

Have no inbound rules but configure outbound rules for all traffic.

## NACL

* Examine the default seting for both inbound and outbound rules in NACL configuration.

* Modify the inbound rules to permit traffic from any IPv4 CIDR on all ports

* add just the outbound rule to allow traffic to all CIDR.

### Part -1

EC2 instance use in hosting droyal1 website

EC2 instance use in hosting kaysolar website.

![](1.%20Instance.png)

* Here's the security group configuration for the instance. In the inbound rules, only IPv4 SSH traffic on port 22 is permitted to access this instance.

![](2.%20Inbound%20rule.png)

* For the outbound rule, you'll notice that all IPv4 traffic with any protocol on any port number is allowed, meaning this instance has unrestricted access to anywhere on the internet.

![](3.%20Outbound%20rule.png)

* Website accessibility using the public IP address assigned to the instance

![](4.%20Public%20IP.png)

* If you enter the IP Address into your Chrome browser, and hit enter, you'll notice that the page doesn't load, it keeps attempting to connect. And finally it'll show this page. After some time, you'll likely see a page indicating that the site can't be reached.

![](5.%20site%20can't%20be%20reached.png)

* This is because of the security group, because we haven't defined HTTP protocol in the security group so whenever the outside world is trying to go inside our instance and trying to get the data, security group is restricting it and that's why we are unable to see the data

* To resolve this issue, we can create a new security group that allows HTTP (port 80) traffic

* Navigate to the 'Security Groups" section on the left sidebar.
Then click on "Create Security Group"

* Provide a name and a description for the new security group
* Select the VPC during your creation process

* Click on add rule

* Select HTTP as the type

* Use 0.0.0.0/0 as the CIDR Block. (Here we are allowing every CIDR block by using this CIDR).

* You will see the rule have been created.

* Keep outbound rule as it is
* Click on create Security group

![](6.%20Creating%20Security%20Group.png)

Security Group created successfully

![](7.%20SG%20created.png)

-Let's attach this security group to our instance. Now navigate to the instance section of left side bar.

* Select the instance.

* Click on "Actions."

* Choose "security.

* Click on change security group

![](8.%20Changing%20SG%20Group.png)

* Choose the security group you created
* Click Add security group

* The security group has been added

**Note** The security group named "Launch Wizard" you see is the default security group automatically attached when creating the instance. You can also edit this security group if needed.

* Click on save

![](9.%20Add%20SG%20group.png)

* Attached successfully

* Copy the IP Address again

![](10.%20Copy%20IP%20again.png)

* Paste the IP Address on chrome to see the output
* You will be able to see the data of our website.

![](13.%20Default%20webPage.png)

Currently, let's take a look at how our inbound and outbound rules are configured.
This setup allows the HTTP and SSH protocols to access the instance

![](11.%20Inbound%20rule.png)

* The outbound rule permits all traffic to exit the instance.

![](12.%20outbound%20rule.png)

* Through this rule we are able to access the website

* let's see how removing the outbound rule affects the instance's connectivity. Means now, no one can go outside to this instance

* Go to outbound tab.

* Click on 'edit outbound rules"

* Click on delete
* Click on save rule

![](14.%20Edit%20Outbound%20rule.png)

* Now that the outbound rule is removed let's take a look at how its appears on the configuration

![](15.%20Successful.png)

* After making the changes let's test and see if we can still access the website

![](13.%20Default%20webPage.png)

So, even though we've removed the outbound rule that allows all traffic from the instance to the outside world, we can still access the website. According to the logic, when a user accesses the instance, the inbound rule permits HTTP protocol traffic to enter. However, when the instance sends data to the user's browser to display the website, the outbound rule should prevent it. Yet, we're still able to view the website. Why might that be?

Security groups are stateful, which means they automatically allow return traffic initiated by the instances to which they are attached. So, even though we removed the outbound rule, the security group allows the return traffic necessary for displaying the website, hence we can still access it.

A scenario,

If we delete both the inbound and outbound rules, essentially, when we're closing all access to and from the instance. This means no traffic can come into the instance, and the instance cannot send any traffic out. So, if we attempt to access the website from a browser or any other client, it will fail because there are no rules permitting traffic to reach the instance. Similarly, the instance won't be able to communicate with any external services or websites because all outbound traffic is also blocked.

Deleting the inbound rule

* Go to inbound tab.

* Click on edit inbound rule

![](16.%20Edit%20Inbound.png)

* Click on delete and Save rule

* Successfully deleted

![](17.%20Successful.png)

* Now, as both the inbound and outbound rules deleted, there's no way for traffic to enter or leave the instance. This means that any attempt to access the website from a browser or any other client will fail because there are no rules permitting traffic to reach the instance. In this state, the instance is essentially isolated from both incoming and outgoing traffic. So you can't access the website now.

![](18.%20Output.png)

In the next scenario,

We'll add a rule specifically allowing HTTP traffic in the outbound rules. This change will enable the instance to initiate outgoing connections over HTTP.

* Click on edit outbound rule in the outbound tab

* Click on add rule
* Choose type
* Choose destination
* choose CIDR
* Click on save rule

![](19.%20Edit%20Outbound.png)

![](20.%20Successful.png)

* Lets see if we can access the website again

![](18.%20Output.png)

So its still unreachable

* Let go to the outside world from instance
* Connect to the EC2 Instance

![](21.%20Git.png)

As a result, the instance will be able to fetch data from external sources or communicate with other HTTP-based services on the internet. This adjustment ensures that while incoming connections to the instance may still be restricted, the instance itself can actively communicate over HTTP to external services.

### Part - 2

Hands-on Network Access Control List (NACL) Examine the default settings for both inbound and outbound rules in NACL configuration.

Modify the inbound rules to permit traffic from any IPV4 CIDR on all ports.

Adjust the outbound rules to allow traffic to all CIDRs.

Navigate to VPC

![](22.%20VPC.png)

* Navigate to the Network ACLs
* Click on create Network ACL

![](23.%20Create%20network%20ACL.png)

* Now, provide a name for your Network ACL,

* Choose the VPC you created, in this case i am using a default VPC

* Then click on "Create network ACL".

![](24.%20Create%20network%20ACL.png)

* Display inbound NACL rule

![](25.%20Inbound%20Rule.png)

* Similarly, if you look at the outbound rules, you'll observe that it's denying all outbound traffic on all ports by default.

* Select the NACL

* And navigate to the "Outbound" tab.

![](26.%20Outbound%20rule.png)

* To make changes
* Select the NACL
* Go to the inbound tab
* Click edit inbound rule

* Click add new rule

![](27.%20Edit%20inbound%20rule.png)

* Currently, the NACL is not associated with any of the subnet in the VPC

![](28.%20NACL%20No%20subnet.png)

* Associate the NACL
* Select the NACL
* Click on Action
* Choose edit subnet association

* Select your public subnet, because the instance reside in the subnet
* Once selected you wil see that its listed under "Selected Subnet"
* Save Change

![](29.%20Asso%20subnet.png)

* Public subnet successfully associated to the NACL

![](30.%20Successful.png)

As soon as you have attached this NACL to your public subnet, and then you try to access the website again by typing the Public IP Address, you will notice that you are unable to see the website.

![](18.%20Output.png)

Although we've permitted all traffic in the inbound rule of our NACL, we're still unable to access the website. This raises the question why isn't the website visible despite these permissions?

The reason why we're unable to access the website despite permitting inbound traffic in the NACL is because NACLs are stateless. They don't automatically allow return traffic. As a result, we must explicitly configure rules for both inbound and outbound traffic.

Even though the inbound rule allows all traffic into the subnet, the outbound rules are still denying all traffic.

If we allow outbound traffic as well,

* Choose you NACL.

* Go to outbound tab.

* Click on "Edit outbound rules."

* Click on Add new rule and save

![](31.%20Outbound%20rule.png)

Upon revisiting the website, it now accessible for the public's use

![](18.%20Output.png)

So we are unable to access the website. why? Even if we have allowed inbound traffic for HTTP in security group.

Imagine you're at the entrance of a building, and there's a security guard checking everyone who wants to come in. That security guard is like the NACL. They have a list of rules (like "no backpacks allowed" or "no food or drinks inside"), and they check each person against these rules as they enter.

Once you're inside the building, there's another layer of security at each room's door. These are like the security group. Each room has its own rules, like "only employees allowed" or "no pets."

These rules are specific to each room, just like security groups are specific to each EC2 instance.

So, the traffic first goes through the NACL (the security guard at the entrance), and if it passes those rules, it then goes through security group (the security check at each room's door). If it doesn't meet any of the rules along the way, it's denied entry.

The reason we can't see the website is because the NACL has denied inbound traffic. This prevents traffic from reaching the security group, much like a security guard not allowing entry to another room if access to the building is denied. Similarly, if someone can't enter a building, they can't access any rooms inside without first gaining entry to the building.

Let's have a look on some scenarios and their outcomes,

NACL allows all inbound and outbound traffic, security group denies all inbound and outbound traffic: Outcome: Website access will be blocked because the Security Group denies all traffic, overriding the NACL's allowance.

NACL denies all inbound and outboud traffic, Security Group allows all inbound and outbound traffic: Outcome: Website access will be blocked because the NACL denies all traffic, regardeless of the security group's allowances.

NACL allows HTTP inbound traffic, outbound traffic is denied, Security Group allows inbound traffic and denies outbound traffic: Outcome: Website access will be allowed because the security group allows HTTP inbound traffic, regardless of the NACLs allowances. However, if the website requires outbound traffic to function properly, it won't work due to the security groups denial of outbound traffic.

NACL allows all inbound and outbound traffic, security group allows HTTP inbound traffic and denies outbound traffic: Outcome: Website access will be allowed because the security group allows HTTP inbound traffic, regardless of the NACL's allowances. However, if the website requires outbound traffic to function properly, it won't work due to the security group's

NACL allows all inbound and outbound traffic, security group allows all inbound and outbound traffic: Outcome: Website access will be allowed, as both NACL and Security Group allow all traffic.

NACL denies all inbound and outbound traffic, Security Group allows HTTP inbound traffic and denies outbound traffic: Outcome: Website access will be block because the NACL denies all traffic, regardless of the security group allowances.





