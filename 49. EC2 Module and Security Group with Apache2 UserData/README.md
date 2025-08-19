# Mini_Project - EC2 Module and Security Group with Apache2 UserData

Purpose:

In this Mini Project we will Use terraform to create modularized configurations for deploying an EC2 instances with a specified security Group and Apache2 installed using UserData.

Objectives:
1. Terraform Module Creation:
- Learn how to create Terraform Modules for modular infrastructure provisioning.

2. EC2 Instance Configuration:
- Configure Terraform to create an EC2 instance.

3. Security Group Configuration:
- Create a seperate module for the security group associated with the EC2 instance.

4. UserData Script:
- Utilize UserData to install and Configure Apache2 on the EC2 instance.

## Project Tasks:
**TASK 1: EC2 Module**
1. Create a new directory for your Terraform project (`terraform-ec2-apache`).

1. Create a directory for your Terraform Project using a terminal
```bash
mkdir terraform-ec2-apache
```

2. Change into the Project directory 
```bash
cd terraform-ec2-apache
```
![](1.%20mkdir.png)

2. Inside the Project directory, Create a directory for the EC2 modules (`modules/ec2`)

![](2.%20mkdir%20SG%20&%20ec2.png)

3. Write a Terraform Module (`modules/ec2/main.tf`) to create an EC2 instance.

![](3.%20ec2%20main.tf.png)

```bash
variable "ami_id" {}
variable "instance_type" {}
variable "subnet_id" {}
variable "security_group_id" {}
variable "key_name" {}
variable "user_data" {}

resource "aws_instance" "this" {
  ami                    = var.ami_id
  instance_type          = var.instance_type
  subnet_id              = var.subnet_id
  vpc_security_group_ids = [var.security_group_id]
  key_name               = var.key_name
  user_data              = file(var.user_data)

  tags = {
    Name = "Apache-EC2"
  }
}

output "public_ip" {
  value = aws_instance.this.public_ip
}

output "public_dns" {
  value = aws_instance.this.public_dns
}
```

**TASK 2: Security Group Module**
1. Inside the project directory, create a directory for the security group module (`modules/security_group`)

![](2.%20mkdir%20SG%20&%20ec2.png)

2. Write a Terraform Module (`modules/security_group/main.tf`) to create a security group for the EC2 instance.

![](4.%20sg%20main.tf.png)

```bash
variable "vpc_id" {}

resource "aws_security_group" "this" {
  name        = "apache-sg"
  description = "Allow SSH and HTTP"
  vpc_id      = var.vpc_id

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "apache-sg"
  }
}

output "security_group_id" {
  value = aws_security_group.this.id
}
```

**TASK 3: UserData Script**
1. Write a UserData Script to install and configure Apache2 on the EC2 instance. Save it as a seperate file (`apache_userdata.sh`).

![](5.%20touch%20userdata.png)

```bash
#!/bin/bash
# Update system and install Apache2
sudo apt-get update -y
sudo apt-get install -y apache2

# Enable and start Apache
sudo systemctl enable apache2
sudo systemctl start apache2

# Create a simple index.html page
echo "<h1>Hello from Apache on EC2 🚀</h1>" | sudo tee /var/www/html/index.html
```

2. Ensure that the UserData Script is executable (`chmod +x apache_userdata.sh`).

![](6.%20chmod.png)

**TASK 4: Main Terraform Configuration 
1. Create the main Terraform configuration file (`main.tf`) in the project directory.

![](7.%20main.tf.png)
```bash
provider "aws" {
  region = "us-east-1"
}

# Get latest Ubuntu AMI
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

# VPC (use default VPC for simplicity)
data "aws_vpc" "default" {
  default = true
}

data "aws_subnet_ids" "default" {
  vpc_id = data.aws_vpc.default.id
}

# Security Group Module
module "security_group" {
  source = "./modules/security_group"
  vpc_id = data.aws_vpc.default.id
}

# EC2 Module
module "ec2" {
  source            = "./modules/ec2"
  ami_id            = data.aws_ami.ubuntu.id
  instance_type     = "t2.micro"
  subnet_id         = element(data.aws_subnet_ids.default.ids, 0)
  security_group_id = module.security_group.security_group_id
  key_name          = "my-key"   # <-- replace with your EC2 key pair
  user_data         = "apache_userdata.sh"
}

output "ec2_public_ip" {
  value = module.ec2.public_ip
}

output "ec2_public_dns" {
  value = module.ec2.public_dns
}
```

2. Use the EC2 and security group modules to create neccesary infrastructure for the EC2 instance.

**TASK 5: Deployment**
1. Run `terraform init` and `terraform apply` to deploy the EC2 instance with Apache2.

![](9.%20terraform%20init.png)

![](10.%20terraform%20validate.png)

![](11.%20terraform%20apply.png)

![](12.%20terraform%20apply.png)

2. Access the EC2 instance and verify that Apache2 is installed and running.

When done, grab the Public IP or DNS:
```bash
terraform output ec2_public_ip
```
![](13.%20Public%20IP.png)

![](14.%20confirmation.png)

**Instructions:**
1. Create a directory for your Terraform Project using a terminal(`mkdir terraform-ec2-apache`).
2. Change into the Project directory (`cd terraform-ec2-apache`)
3. Create directories for the EC2 and security group modules (`mkdir -p modules/ec2` and `mkdir -p modules/security_group`)
4. Write the EC2 module configuration (`nano modules/ec2/main.tf`) to create an EC2 instance.
5. Write the Security Group Module Configuration (`nano modules/security_group/main.tf`) to create a security group.
6. Write the UserData Script (`nano apache_userdata.sh`) to install and configure Apache2. 
```bash
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" | sudo tee /var/www/html/index.html
```
7. Make the UserData Script Executable (`chmod +x apache_userdata.sh`)
8. Create the main terraform configuration file (`nano main.tf`) and use the EC2 and Security Group modules.
```bash
module "security_group" {"\n  source = \"./modules/security_group\"\n  // Add variables for customizing the Security Group if needed\n"}

module "ec2_instance" {"\n  source          = \"./modules/ec2\"\n  security_group_id = module.security_group.security_group_id\n  user_data       = file(\"apache_userdata.sh\")\n  // Add other variables as needed\n"}
```
9. Run `terraform init` and `terraform apply` to deploy the EC2 intance with Apache2.
10. Access the EC2 instance using the Public IP and verify that the Apache2 is installed and running.
11. Document your observations and any challenges faced during the Project.

**SIDE NOTES:** 
```bash
- Ensure you have the AWS CLI installed and configured with appropriate credentials.
- Modify variables and configurations in the modules based on your specific requirements.
- This is a learning exercise; use it to gain hands-on experience with Terraform, EC2, UserData, and Security Groups.
```