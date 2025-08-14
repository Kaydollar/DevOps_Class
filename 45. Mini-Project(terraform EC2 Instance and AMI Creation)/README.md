# Mini Project- Terraform EC2 Instance and AMI Creation

Objectives: 
- Learn how to write Basic Terraform Configuration files.
- Learn how to write Terraform Script to automate creation of EC2 Instances on AWS.
- Learn how to use Terraform Script to automate the creation of AMI from an already created EC2 instance on AWS.

Pre-requisites
- Ensure you have an AWS account created and Functional. see guide [here](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html) to creare an AWS account.
- Ensure you have the AWS CLI installed and configured with your AWS Account credentials. you may see guide [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) .
- Ensure you have terraform installed in your computer.

## Task Outlines
1. Confirm Pre-requisites
2. Write the Script
3. Execute the Script
    1. Initialize[init]
    2. Validate[validate]
    3. Plan[plan]
    4. Apply[apply]
4. Confirm Resources
5. Clean UP
    1. Destroy[destroy]

### Project Task
 Task 1 - Confirm the Pre-requisites
1. Login into AWS account to confirm it is functional
![](1.%20Confirm%20AWS.png)

2. Run `aws --version` on your terminal to confirm AWS CLI is installed.
![](2.%20AWS%20CLI.png)

3. Run `aws configure` to confirm AWS CLI is configured.
![](3.%20AWS%20CLI%20configured.png)

4. Run `aws sts get-caller-identity` to verify AWS CLI can successfully authenticate to your AWS Account.
![](4.%20get%20caller.png)

5. Run `terraform --version` to confirm terraform is installed. 
![](5.%20Terraform%20Installed.png)

TASK 2

1. Create a new directory for this Terraform Project: `mkdir terraform-ec2-ami` and `cd terraform-ec2-ami`
![](6.%20mkdir%20cd.png)

2. Inside this directory, create a Terraform configuration file `nano main.tf`
![](7.%20main.tf.png)

3. Write the script to create EC2 instance specifying instance type, ami and tag. Extend the script to include the creation of an AMI from the created EC2 instance.
```bash
provider "aws" {
  region = "us-east-1" # Change this to your AWS region
}

# Create EC2 instance
resource "aws_instance" "my_ec2_spec" {
  ami           = "ami-0c55b159cbfafe1f0" # Example Amazon Linux 2 AMI for us-east-1
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-created-EC2-Instance"
  }
}

# Create AMI from the EC2 instance
resource "aws_ami_from_instance" "my_ec2_spec_ami" {
  name               = "my-ec2-ami"
  description        = "My AMI created from my EC2 Instance with Terraform script"
  source_instance_id = aws_instance.my_ec2_spec.id

  tags = {
    Name = "Terraform-created-AMI"
  }
}
```
![](8.%20main.tf.png)

Script Explanation

This script creates an EC2 instance and then creates an AMI from that instance.

1. Provider Block
    1. `provider "aws"` tells Terraform to use AWS as the cloud provider
    2. `region = "us-east-1"` specifies which AWS region to use
2. EC2 Instance Creation
    1. `resource "aws_instance" "my_ec2_spec"` creates an EC2 Instance
    2. `ami = "ami-0c55b159cbfafe1f0"` specifies the Amazon Machine Image ID to use for the instance
    3. `instance_type = "t2.micro"` defines the EC2 Instance type
    4. The `tag` block adds a name tag to the instance for identification
3. AMI Creation from the EC2 Instance
    1. `resource "aws_ami" "my_ec2_spec_ami"` creates an AMI from the EC2 Instance
    2.  `name = "my-ec2-ami"` names the new AMI
    3. `source_instance_id = aws_instance.my_ec2_spec.id` uses the EC2 Instance to create the AMI

**TASK 3** Executing the terraform Script
1. Initialize the Terraform Project using `terraform init`
![](9.%20terraform%20init.png)

2. Validate the correctness of the script using `terraform validate`
![](10.%20terraform%20validate.png)

3. Confirm the resources that will be created by executing the script using `terraform plan`
![](11.%20terraform%20plan.png)

![](12.%20terraform%20plan.png)

4. Apply the terraform configuration using `terraform apply`
![](13.%20terraform%20apply.png)

![](14.%20terraform%20apply.png)

**TASK 4: Confirm the Process**
- Confirm the creation of the EC2 instance and its AMI in your AWS account according to the specified details. 
![](15.%20confirmed.png)

**TASK 5: Clean UP**
- Execute command `terraform destroy` to clean up the resources created by the script.
![](16.%20terraform%20destroy.png)

![](17.%20terraform%20destroy.png)

![](18.%20Instance%20destroyed.png)

