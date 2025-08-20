# Terraform Capstone Project - Automated WordPress Deployment on AWS

**Project Scenerio:**

DigitalBoost, a digital marketing agency, aims to elevate its online presence by launching a high- performance wordpress website for their client. As an AWS solutions Architect, your task is to design and implement a scalable, secure and cost-effective WordPress Solution using various AWS services. Automation through Terraform will be a key to archieving a streamlined and reproducable deployment process. 

**Pre-requisites:**

- Knowledge of TechOps Essentials
- Completion of Core 2 coursesand Mini Projects.

The project Overview, neccesary architecture, and scripts have been provided to help DigitalBoost with their WordPress based website. 

**Project Deliverables:**
- Documentation:
    - Detailed Documentation for each component setup
    - Explanation of Security Measures implemented.

- Demonstration:
    - Live Demonstration of WordPress Site
    - Showcase auto-scalling by simulating increased traffic.

- Project Overview:
![](./1.%20PO%201.png)

- Project Components:
1. VPC Setup

    VPC Architecture

![](2.%20PO%202.png)

- Objective: Create a Virtual Private Cloud (VPC) to isolate and secure the WordPress infrastructure.

- Steps:
    - Define IP address range for the VPC
    - Create VPC with public and private subnets
    - Configure route table for each subnets.

- Instructure for Terraform:
    - Use terraform to define VPC, Subnets and route tables.
    - Leverage variables for customization 
    - Document Terraform command for execution

2. Public and Private Subnet with NAT Gateway

**NAT GATEWAY ARCHITECTURE**

![](3.%20PO%203.png)

- Objective: Implement a secure network architecture with public and private subnets. Use a NAT Gateway for Private subnet internet access.
- Steps:
    - Set up a Public subnet for resource accessible from the internet.
    - Create a private subnet for resources with no direct internet access.
    - Configure a NAT gateway for private subnet internet access.

**Instruction for Terraform**
- Utilize terraform to define subnets, security group and NAT gateway.
- Ensure proper association of resources with corresponding subnets.
- Document Terraform commands for execution.

3. AWS MYSQL RDS Setup

    **SECURITY GROUP ARCHITECTURE**

![](4.%20PO%204.png)

- Objective: Deploy a managed MySQL database using Amazon RDS for WordPress data storage.
- Steps:
    - Create an Amazon RDS instance with the MySQL engine..
    - A Configure security groups for the RDS instance.
    - Connect WordPress to the RDS database.

- Instructions for Terraform:
    -  Define Terraform scripts for RDS instance creation.
    - Configure security groups and define necessary parameters.
    - Document Terraform commands for execution.

4. EFS Setup for WordPress Files
- Objective: Utilize Amazon Elastic File System (EFS) to store WordPress files for scalable and shared access.
- Steps:
    - Create an EFS file system.
    - Mount the EFS file system on WordPress instances.
    - Configure WordPress to use the shared file system.

- Instructions for Terraform:
    - Develop Terraform scripts to create EFS file system.
    - Define configurations for mounting EFS on WordPress instances.
    - Document Terraform commands for execution.

5. Application Load Balancer
- Objective: Set up an Application Load Balancer to distribute incoming traffic among multiple instances, ensuring high availability and fault tolerance.
- Steps:
    - Create an Application Load Balancer.
    - Configure listener rules for routing traffic to instances.
    - Integrate Load Balancer with Auto Scaling group.

- Instructions for Terraform:
    - Use Terraform to define Application Load Balancer configurations.
    - Integrate Load Balancer with Auto Scaling group
    - Document Terraform commands for execution.

6. Auto Scaling Group
- Objective: Implement Auto Scaling to automatically adjust the number of instances based on traffic load.
- Steps:
    - Create an Auto Scaling group.
    - Define scaling policies based on metrics like CPU utilization.
    - Configure launch configurations for instances.

- Instructions for Terraform:
    - Develop Terraform scripts for Auto Scaling group creation.
    - Define scaling policies and launch configurations.
    - Document Terraform commands for execution.

**Note:**

Provide thorough documentation for each Terraform script and include necessary variable configurations. Encourage students to perform a live domonstration of the WordPress site, showcasing auto-scaling capabilities by simulating increased traffic. The documentation should explain the security measures implemented at each step.

## Documentation and Deliverables

**Step 1:**

Create the project skeleton:

Open PowerShell (recommended on Windows):
```bash
mkdir digitalboost-wp; cd digitalboost-wp
```
```bash
mkdir modules\vpc, modules\security, modules\rds, modules\efs, modules\alb, modules\asg, user_data, env
```
![](5.%20mkdir.png)

**Step 2:**
- Root files `variables.tf`
```bash
variable "project_name" { type = string }
variable "region"       { type = string }

variable "azs"                 { type = list(string) }
variable "vpc_cidr"            { type = string }
variable "public_subnets"      { type = list(string) }
variable "private_app_subnets" { type = list(string) }
variable "private_db_subnets"  { type = list(string) }

variable "my_ip_cidr" { type = string }         # e.g., "203.0.113.5/32"
variable "key_name"   { type = string }

# WordPress/RDS settings
variable "db_name"      { type = string }
variable "db_username"  { type = string }
variable "db_password"  { type = string }       # set via TF_VAR_db_password
variable "rds_multi_az" { type = bool   default = false }

variable "wp_instance_type" { type = string default = "t3.micro" }

variable "tags" {
  type = map(string)
  default = {}
}
```
- Root file `outputs.tf`
```bash
output "alb_dns"      { value = module.alb.dns_name }
output "rds_endpoint" { value = module.rds.endpoint }
output "efs_id"       { value = module.efs.id }
output "asg_name"     { value = module.asg.asg_name }
```

- Root File `main.tf`
```bash
terraform {
  required_version = ">= 1.3.0"
  required_providers {
    aws = { source = "hashicorp/aws", version = "~> 5.0" }
  }
}

provider "aws" {
  region = var.region
}

locals {
  name = var.project_name

  # Render user-data with DB + EFS values (RDS and EFS defined below)
  user_data = templatefile("${path.module}/user_data/wordpress.sh", {
    db_name     = var.db_name
    db_user     = var.db_username
    db_password = var.db_password
    db_host     = module.rds.endpoint
    efs_id      = module.efs.id
  })
}

module "vpc" {
  source               = "./modules/vpc"
  name                 = local.name
  vpc_cidr             = var.vpc_cidr
  azs                  = var.azs
  public_subnets       = var.public_subnets
  private_app_subnets  = var.private_app_subnets
  private_db_subnets   = var.private_db_subnets
  single_nat_gateway   = true
  tags                 = var.tags
}

module "security" {
  source   = "./modules/security"
  name     = local.name
  vpc_id   = module.vpc.vpc_id
  my_ip    = var.my_ip_cidr
  tags     = var.tags
}

module "efs" {
  source            = "./modules/efs"
  name              = local.name
  subnets           = module.vpc.private_app_subnet_ids
  security_group_id = module.security.efs_sg_id
  tags              = var.tags
}

module "rds" {
  source                = "./modules/rds"
  name                  = local.name
  subnets               = module.vpc.private_db_subnet_ids
  vpc_security_group_ids = [module.security.db_sg_id]
  db_name               = var.db_name
  db_username           = var.db_username
  db_password           = var.db_password
  multi_az              = var.rds_multi_az
  tags                  = var.tags
}

module "alb" {
  source            = "./modules/alb"
  name              = local.name
  vpc_id            = module.vpc.vpc_id
  subnets           = module.vpc.public_subnet_ids
  security_group_id = module.security.alb_sg_id
  tags              = var.tags
}

module "asg" {
  source                = "./modules/asg"
  name                  = local.name
  subnets               = module.vpc.private_app_subnet_ids
  security_group_id     = module.security.web_sg_id
  alb_target_group_arn  = module.alb.target_group_arn
  key_name              = var.key_name
  instance_type         = var.wp_instance_type
  user_data             = local.user_data
  tags                  = var.tags
}
```
![](6.%20root%20file.png)

**Step 3:**
User-data (WordPress bootstrap)
`user_data/wordpress.sh`
```bash
#!/bin/bash
set -euxo pipefail

# ---------- Inputs from templatefile ----------
DB_NAME="${db_name}"
DB_USER="${db_user}"
DB_PASSWORD="${db_password}"
DB_HOST="${db_host}"
EFS_ID="${efs_id}"

# ---------- System prep ----------
yum update -y
amazon-linux-extras enable php8.2
yum install -y httpd php php-mysqli php-json php-gd php-zip php-mbstring php-opcache php-xml curl tar unzip amazon-efs-utils

systemctl enable httpd
systemctl start httpd

# ---------- EFS mount ----------
mkdir -p /var/www/html
echo "${EFS_ID}.efs.${AWS_DEFAULT_REGION:-$(curl -s http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r .region)}.amazonaws.com:/ /var/www/html efs _netdev,tls 0 0" >> /etc/fstab || true
mount -a || true

# If EFS is empty, fetch WordPress
if [ ! -f /var/www/html/wp-config.php ]; then
  curl -L -o /tmp/wordpress.tar.gz https://wordpress.org/latest.tar.gz
  tar -xzf /tmp/wordpress.tar.gz -C /tmp
  rsync -a /tmp/wordpress/ /var/www/html/
  chown -R apache:apache /var/www/html
fi

# ---------- wp-config.php ----------
if [ ! -f /var/www/html/wp-config.php ]; then
  cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
  perl -pi -e "s/database_name_here/${DB_NAME}/" /var/www/html/wp-config.php
  perl -pi -e "s/username_here/${DB_USER}/" /var/www/html/wp-config.php
  perl -pi -e "s/password_here/${DB_PASSWORD}/" /var/www/html/wp-config.php
  perl -pi -e "s/localhost/${DB_HOST}/" /var/www/html/wp-config.php

  # Secure salts
  SALTS=$(curl -s https://api.wordpress.org/secret-key/1.1/salt/)
  perl -0777 -pe "s/define\('AUTH_KEY'.*?define\('NONCE_SALT'.*?;/${SALTS}/s" -i /var/www/html/wp-config.php

  chown -R apache:apache /var/www/html
  find /var/www/html -type d -exec chmod 755 {} \;
  find /var/www/html -type f -exec chmod 644 {} \;
fi

systemctl restart httpd
```

**Step 4:** 
Modules (minimal, production-sane)
`modules/vpc/main.tf`
```bash
variable "name" {}
variable "vpc_cidr" {}
variable "azs" { type = list(string) }
variable "public_subnets" { type = list(string) }
variable "private_app_subnets" { type = list(string) }
variable "private_db_subnets" { type = list(string) }
variable "single_nat_gateway" { type = bool default = true }
variable "tags" { type = map(string) default = {} }

resource "aws_vpc" "this" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  tags = merge(var.tags, { Name = "${var.name}-vpc" })
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.this.id
  tags   = merge(var.tags, { Name = "${var.name}-igw" })
}

resource "aws_subnet" "public" {
  for_each                = zipmap(var.azs, var.public_subnets)
  vpc_id                  = aws_vpc.this.id
  cidr_block              = each.value
  availability_zone       = each.key
  map_public_ip_on_launch = true
  tags = merge(var.tags, { Name = "${var.name}-public-${each.key}" })
}

resource "aws_subnet" "app" {
  for_each          = zipmap(var.azs, var.private_app_subnets)
  vpc_id            = aws_vpc.this.id
  cidr_block        = each.value
  availability_zone = each.key
  tags = merge(var.tags, { Name = "${var.name}-app-${each.key}" })
}

resource "aws_subnet" "db" {
  for_each          = zipmap(var.azs, var.private_db_subnets)
  vpc_id            = aws_vpc.this.id
  cidr_block        = each.value
  availability_zone = each.key
  tags = merge(var.tags, { Name = "${var.name}-db-${each.key}" })
}

# NAT (one per VPC to save cost)
resource "aws_eip" "nat" { vpc = true }
resource "aws_nat_gateway" "nat" {
  allocation_id = aws_eip_nat_id()
  subnet_id     = values(aws_subnet.public)[0].id
  tags          = merge(var.tags, { Name = "${var.name}-nat" })
}
# helper to avoid count/for_each id snag
function "aws_eip_nat_id"() { result = aws_eip.nat.id }

# Public RT
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.this.id
  route { cidr_block = "0.0.0.0/0"; gateway_id = aws_internet_gateway.igw.id }
  tags = merge(var.tags, { Name = "${var.name}-public-rt" })
}

resource "aws_route_table_association" "public" {
  for_each       = aws_subnet.public
  subnet_id      = each.value.id
  route_table_id = aws_route_table.public.id
}

# Private RT (via NAT)
resource "aws_route_table" "private" {
  vpc_id = aws_vpc.this.id
  route { cidr_block = "0.0.0.0/0"; nat_gateway_id = aws_nat_gateway.nat.id }
  tags = merge(var.tags, { Name = "${var.name}-private-rt" })
}

resource "aws_route_table_association" "app" {
  for_each       = aws_subnet.app
  subnet_id      = each.value.id
  route_table_id = aws_route_table.private.id
}
resource "aws_route_table_association" "db" {
  for_each       = aws_subnet.db
  subnet_id      = each.value.id
  route_table_id = aws_route_table.private.id
}

output "vpc_id"                 { value = aws_vpc.this.id }
output "public_subnet_ids"      { value = [for s in aws_subnet.public : s.id] }
output "private_app_subnet_ids" { value = [for s in aws_subnet.app    : s.id] }
output "private_db_subnet_ids"  { value = [for s in aws_subnet.db     : s.id] }
```
`modules/security/main.tf`
```bash
variable "name" {}
variable "vpc_id" {}
variable "my_ip" {}
variable "tags" { type = map(string) default = {} }

resource "aws_security_group" "alb" {
  name   = "${var.name}-alb-sg"
  vpc_id = var.vpc_id
  ingress { from_port = 80  to_port = 80  protocol = "tcp" cidr_blocks = ["0.0.0.0/0"] }
  # add 443 later when you attach ACM cert
  egress  { from_port = 0  to_port = 0   protocol = "-1" cidr_blocks = ["0.0.0.0/0"] }
  tags = merge(var.tags, { Name = "${var.name}-alb-sg" })
}

resource "aws_security_group" "web" {
  name   = "${var.name}-web-sg"
  vpc_id = var.vpc_id
  # ALB -> Web on 80
  ingress { from_port = 80 to_port = 80 protocol = "tcp" security_groups = [aws_security_group.alb.id] }
  # Optional: SSH from your IP
  ingress { from_port = 22 to_port = 22 protocol = "tcp" cidr_blocks = [var.my_ip] }
  egress  { from_port = 0  to_port = 0  protocol = "-1" cidr_blocks = ["0.0.0.0/0"] }
  tags = merge(var.tags, { Name = "${var.name}-web-sg" })
}

resource "aws_security_group" "db" {
  name   = "${var.name}-db-sg"
  vpc_id = var.vpc_id
  ingress { from_port = 3306 to_port = 3306 protocol = "tcp" security_groups = [aws_security_group.web.id] }
  egress  { from_port = 0    to_port = 0    protocol = "-1" cidr_blocks = ["0.0.0.0/0"] }
  tags = merge(var.tags, { Name = "${var.name}-db-sg" })
}

resource "aws_security_group" "efs" {
  name   = "${var.name}-efs-sg"
  vpc_id = var.vpc_id
  ingress { from_port = 2049 to_port = 2049 protocol = "tcp" security_groups = [aws_security_group.web.id] }
  egress  { from_port = 0    to_port = 0    protocol = "-1" cidr_blocks = ["0.0.0.0/0"] }
  tags = merge(var.tags, { Name = "${var.name}-efs-sg" })
}

output "alb_sg_id" { value = aws_security_group.alb.id }
output "web_sg_id" { value = aws_security_group.web.id }
output "db_sg_id"  { value = aws_security_group.db.id }
output "efs_sg_id" { value = aws_security_group.efs.id }
```

`modules/efs/main.tf`
```bash
variable "name" {}
variable "subnets" { type = list(string) }
variable "security_group_id" {}
variable "tags" { type = map(string) default = {} }

resource "aws_efs_file_system" "this" {
  encrypted = true
  tags = merge(var.tags, { Name = "${var.name}-efs" })
}

resource "aws_efs_mount_target" "mt" {
  for_each       = toset(var.subnets)
  file_system_id = aws_efs_file_system.this.id
  subnet_id      = each.value
  security_groups = [var.security_group_id]
}

output "id" { value = aws_efs_file_system.this.id }
```

`modules/rds/main.tf`
```bash
variable "name" {}
variable "subnets" { type = list(string) }
variable "vpc_security_group_ids" { type = list(string) }
variable "db_name" {}
variable "db_username" {}
variable "db_password" {}
variable "multi_az" { type = bool default = false }
variable "tags" { type = map(string) default = {} }

resource "aws_db_subnet_group" "this" {
  name       = "${var.name}-db-subnets"
  subnet_ids = var.subnets
  tags       = merge(var.tags, { Name = "${var.name}-db-subnets" })
}

resource "aws_db_instance" "this" {
  identifier              = "${var.name}-mysql"
  engine                  = "mysql"
  engine_version          = "8.0"
  instance_class          = "db.t3.micro"
  allocated_storage       = 20
  db_name                 = var.db_name
  username                = var.db_username
  password                = var.db_password
  db_subnet_group_name    = aws_db_subnet_group.this.name
  vpc_security_group_ids  = var.vpc_security_group_ids
  multi_az                = var.multi_az
  skip_final_snapshot     = true
  backup_retention_period = 7
  deletion_protection     = false
  publicly_accessible     = false
  storage_encrypted       = true
  tags                    = merge(var.tags, { Name = "${var.name}-rds" })
}

output "endpoint" { value = aws_db_instance.this.address }
```

`modules/alb/main.tf`
```bash
variable "name" {}
variable "vpc_id" {}
variable "subnets" { type = list(string) }
variable "security_group_id" {}
variable "tags" { type = map(string) default = {} }

resource "aws_lb" "this" {
  name               = "${var.name}-alb"
  load_balancer_type = "application"
  subnets            = var.subnets
  security_groups    = [var.security_group_id]
  idle_timeout       = 60
  tags               = merge(var.tags, { Name = "${var.name}-alb" })
}

resource "aws_lb_target_group" "tg" {
  name        = "${var.name}-tg"
  port        = 80
  protocol    = "HTTP"
  vpc_id      = var.vpc_id
  target_type = "instance"
  health_check {
    path                = "/"
    matcher             = "200-399"
    interval            = 30
    healthy_threshold   = 3
    unhealthy_threshold = 3
    timeout             = 5
  }
  tags = merge(var.tags, { Name = "${var.name}-tg" })
}

resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.this.arn
  port              = 80
  protocol          = "HTTP"
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tg.arn
  }
}

output "dns_name"         { value = aws_lb.this.dns_name }
output "target_group_arn" { value = aws_lb_target_group.tg.arn }
```

`modules/asg/main.tf`
```bash
variable "name" {}
variable "subnets" { type = list(string) }
variable "security_group_id" {}
variable "alb_target_group_arn" {}
variable "key_name" {}
variable "instance_type" {}
variable "user_data" {}
variable "tags" { type = map(string) default = {} }

# Latest Amazon Linux 2
data "aws_ami" "al2" {
  most_recent = true
  owners      = ["137112412989"]
  filter { name = "name"; values = ["amzn2-ami-hvm-*-x86_64-gp2"] }
}

# (Optional but handy) SSM access for troubleshooting
resource "aws_iam_role" "ec2_role" {
  name = "${var.name}-ec2-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{Effect="Allow", Principal={Service="ec2.amazonaws.com"}, Action="sts:AssumeRole"}]
  })
}
resource "aws_iam_role_policy_attachment" "ssm" {
  role       = aws_iam_role.ec2_role.name
  policy_arn = "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"
}
resource "aws_iam_instance_profile" "ec2_profile" {
  name = "${var.name}-ec2-profile"
  role = aws_iam_role.ec2_role.name
}

resource "aws_launch_template" "lt" {
  name_prefix   = "${var.name}-lt-"
  image_id      = data.aws_ami.al2.id
  instance_type = var.instance_type
  key_name      = var.key_name

  vpc_security_group_ids = [var.security_group_id]
  user_data              = base64encode(var.user_data)

  iam_instance_profile { name = aws_iam_instance_profile.ec2_profile.name }

  tag_specifications {
    resource_type = "instance"
    tags          = merge(var.tags, { Name = "${var.name}-wp" })
  }
}

resource "aws_autoscaling_group" "asg" {
  name                      = "${var.name}-asg"
  desired_capacity          = 2
  min_size                  = 2
  max_size                  = 4
  vpc_zone_identifier       = var.subnets
  health_check_type         = "ELB"
  health_check_grace_period = 120

  launch_template {
    id      = aws_launch_template.lt.id
    version = "$Latest"
  }

  target_group_arns = [var.alb_target_group_arn]

  tag {
    key                 = "Name"
    value               = "${var.name}-wp"
    propagate_at_launch = true
  }
}

output "asg_name" { value = aws_autoscaling_group.asg.name }
```

**Step 5:**
Environment values

`env/dev.tfvars` (edit the IP & key_name)
```bash
project_name   = "digitalboost-wp"
region         = "us-east-1"
azs            = ["us-east-1a", "us-east-1b"]

vpc_cidr            = "10.0.0.0/16"
public_subnets      = ["10.0.1.0/24", "10.0.2.0/24"]
private_app_subnets = ["10.0.11.0/24", "10.0.12.0/24"]
private_db_subnets  = ["10.0.21.0/24", "10.0.22.0/24"]

my_ip_cidr     = "YOUR.PUBLIC.IP/32"
key_name       = "digitalboost-key"

db_name        = "wordpress"
db_username    = "wpuser"
# db_password comes from env (safer)
rds_multi_az   = false

wp_instance_type = "t3.micro"

tags = {
  Project = "DigitalBoost"
  Env     = "dev"
}
```

**Step 6:**
Deploy (Windows PowerShell)
```bash
cd digitalboost-wp
$env:TF_VAR_db_password = "StrongPassw0rd!"             # set once per session
terraform init
![](7.%20terraform%20init.png)

terraform workspace new dev 2>$null; terraform workspace select dev


terraform plan -out=tfplan -var-file="env/dev.tfvars"
terraform apply tfplan
```

