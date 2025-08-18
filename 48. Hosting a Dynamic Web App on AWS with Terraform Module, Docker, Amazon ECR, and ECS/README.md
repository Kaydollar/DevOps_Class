# Hosting a Dynamic Web App on AWS with Terraform Module, Docker, Amazon ECR, and ECS

**Purpose:**

In this mini project, you will use Terraform to create a modular infrastructure for hosting a dynamic web application on Amazon ECS (Elastic Container Service). The project involves containerizing the web app using Docker, pushing the Docker image to Amazon ECR (Elastic Container Registry), and deploying the app on ECS.

**Objectives:**

1. Terraform Module Creation:
- Learn how to create Terraform modules for modular infrastructure provisioning.

2. Dockerization:
- Containerize a dynamic web application using Docker.

3. Amazon ECR Configuration:
- Configure Terraform to create an Amazon ECR repository for storing Docker images.

4. Amazon ECS Deployment:
- Use Terraform to provision an ECS cluster and deploy the Dockerized web app.

**Project Tasks:**

## Task 1: Dockerization of Web App
1. Create a dynamic web application using a technology of your choice (eg., Nodejs, Flask, Django).

2. Write a "Dockerfile to containerize the web application.

3. Test the Docker image locally to ensure the web app runs successfully within a container.

- Step 1: Create Project Files

Inside your project folder (e.g., my-webapp), create these three files:
![](2.%20ls.png)

package.json
```bash
{
  "name": "my-webapp",
  "version": "1.0.0",
  "description": "Simple Node.js app running in Docker",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

app.js
```bash
const express = require("express");
const app = express();
const port = 80; // run inside container on port 80

app.get("/", (req, res) => {
  res.send("🚀 Hello from my Dockerized Node.js Web App!");
});

app.listen(port, () => {
  console.log(`App running on port ${port}`);
});
```

Dockerfile
```bash
# Use official Node.js image
FROM node:18-alpine

# Set working directory
WORKDIR /usr/src/app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install --only=production

# Copy app source code
COPY . .

# Expose port
EXPOSE 80

# Start the app
CMD ["npm", "start"]
```

Step 2: Build Docker image
```bash
docker build -t my-webapp .
```
![](1.%20DockerBuild.png)

Step 3: Run container

```bash
docker run -d -p 8080:80 --name my-webapp-container my-webapp
```
![](3.%20docker%20run.png)

Step 4: Test
- Open browser → http://localhost:8080

You should see:
```bash
🚀 Hello from my Dockerized Node.js Web App!
```
![](4.%20Hello.png)


## Task 2: Terraform Module for Amazon ECR
1. Create a new directory for your Terraform project (e.g., `terraform-ecs-webapp`).

2. Inside the project directory, create a directory for the Amazon ECR module (eg, `modules/ecr`).

3. Write a Terraform module (`modules/ecr/main.tf`) to create an Amazon ECR repository for storing Docker images.

Step 1: Create Terraform Project Directory

In your terminal or PowerShell, run:
```bash
mkdir terraform-ecs-webapp
cd terraform-ecs-webapp
```
![](5.%20mkdir.png)

Step 2: Create ECR Module Directory

```bash
mkdir -p modules/ecr
```
![](6.%20mkdir.png)

Step 3: Write the ECR Terraform Module

Create the file:
```bash
nano modules/ecr/main.tf
```

Paste this code inside:
```bash
# modules/ecr/main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

resource "aws_ecr_repository" "this" {
  name                 = var.repository_name
  image_tag_mutability = "MUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }

  encryption_configuration {
    encryption_type = "AES256"
  }

  tags = {
    Name = var.repository_name
  }
}

output "repository_url" {
  value = aws_ecr_repository.this.repository_url
}
```

Step 4: Add Variables File

Create:
```bash
nano modules/ecr/variables.tf
```
```bash
variable "repository_name" {
  description = "Name of the ECR repository"
  type        = string
}
```
![](8.%20ls.png)

## Task 3: Terraform Module for ECS

1. Inside the project directory, create a directory for the ECS module (e.g., `modules/ecs`).

2. Write a Terraform module (`modules/ecs/main.tf`) to provision an ECS cluster and deploy the Dockerized web app

Step 1: Create ECS Module Directory
```bash
mkdir -p modules/ecs
```
![](9.%20mkdir%20ecs.png)

Step 2: Create modules/ecs/main.tf

Paste this code:
```bash
# modules/ecs/main.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# 1. ECS Cluster
resource "aws_ecs_cluster" "this" {
  name = var.cluster_name
}

# 2. IAM Role for ECS Task Execution
resource "aws_iam_role" "ecs_task_execution_role" {
  name = "${var.cluster_name}-ecs-task-exec-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = {
        Service = "ecs-tasks.amazonaws.com"
      }
      Action = "sts:AssumeRole"
    }]
  })
}

# Attach AmazonECSTaskExecutionRolePolicy
resource "aws_iam_role_policy_attachment" "ecs_task_exec_attach" {
  role       = aws_iam_role.ecs_task_execution_role.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
}

# 3. ECS Task Definition
resource "aws_ecs_task_definition" "this" {
  family                   = "${var.cluster_name}-task"
  network_mode             = "awsvpc"
  requires_compatibilities = ["FARGATE"]
  cpu                      = var.task_cpu
  memory                   = var.task_memory
  execution_role_arn       = aws_iam_role.ecs_task_execution_role.arn

  container_definitions = jsonencode([
    {
      name      = "app"
      image     = "${var.ecr_repository_url}:latest"
      essential = true
      portMappings = [
        {
          containerPort = 80
          hostPort      = 80
        }
      ]
    }
  ])
}

# 4. ECS Service
resource "aws_ecs_service" "this" {
  name            = "${var.cluster_name}-service"
  cluster         = aws_ecs_cluster.this.id
  task_definition = aws_ecs_task_definition.this.arn
  launch_type     = "FARGATE"
  desired_count   = 1

  network_configuration {
    subnets         = var.public_subnet_ids
    assign_public_ip = true
    security_groups  = [aws_security_group.ecs_tasks.id]
  }

  load_balancer {
    target_group_arn = aws_lb_target_group.this.arn
    container_name   = "app"
    container_port   = 80
  }

  depends_on = [aws_lb_listener.this]
}

# 5. Security Group for ECS Tasks
resource "aws_security_group" "ecs_tasks" {
  name        = "${var.cluster_name}-ecs-sg"
  description = "Allow HTTP traffic to ECS tasks"
  vpc_id      = var.vpc_id

  ingress {
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
}

# 6. Application Load Balancer
resource "aws_lb" "this" {
  name               = "${var.cluster_name}-alb"
  load_balancer_type = "application"
  subnets            = var.public_subnet_ids
  security_groups    = [aws_security_group.ecs_tasks.id]
}

# 7. Target Group
resource "aws_lb_target_group" "this" {
  name     = "${var.cluster_name}-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = var.vpc_id

  health_check {
    path = "/"
  }
}

# 8. Listener
resource "aws_lb_listener" "this" {
  load_balancer_arn = aws_lb.this.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.this.arn
  }
}

# Outputs
output "ecs_cluster_id" {
  value = aws_ecs_cluster.this.id
}

output "ecs_service_name" {
  value = aws_ecs_service.this.name
}

output "alb_dns_name" {
  value = aws_lb.this.dns_name
}
```
Step 3: Add Variables
```
nano modules/ecs/variables.tf
```
Paste:
```bash
variable "cluster_name" {
  description = "ECS Cluster name"
  type        = string
}

variable "ecr_repository_url" {
  description = "ECR repository URL for the Docker image"
  type        = string
}

variable "vpc_id" {
  description = "VPC ID where ECS will run"
  type        = string
}

variable "public_subnet_ids" {
  description = "List of public subnet IDs"
  type        = list(string)
}

variable "task_cpu" {
  description = "CPU units for the ECS task"
  type        = string
  default     = "256"
}

variable "task_memory" {
  description = "Memory for the ECS task"
  type        = string
  default     = "512"
}
```
![](10.%20ls.png)

## Task 4: Main Terraform Configuration

Step 1: Root Project Structure

My project should look like this:

![](11.%20Project%20structure.png)

1. Create the main Terraform configuration fle (`main. tf`) in the project directory.

2. Use the ECR and ECS modules to create the necessary infrastructure for hosting the web app.

Step 2: Root main.tf

Inside modules/vpc/main.tf:
```bash
# modules/vpc/main.tf

resource "aws_vpc" "this" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "webapp-vpc"
  }
}

resource "aws_internet_gateway" "this" {
  vpc_id = aws_vpc.this.id

  tags = {
    Name = "webapp-igw"
  }
}

resource "aws_subnet" "public" {
  count = 2
  vpc_id                  = aws_vpc.this.id
  cidr_block              = cidrsubnet(aws_vpc.this.cidr_block, 8, count.index)
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true

  tags = {
    Name = "webapp-public-subnet-${count.index}"
  }
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.this.id

  tags = {
    Name = "webapp-public-rt"
  }
}

resource "aws_route" "public_internet_access" {
  route_table_id         = aws_route_table.public.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.this.id
}

resource "aws_route_table_association" "public" {
  count          = length(aws_subnet.public[*].id)
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

data "aws_availability_zones" "available" {}

output "vpc_id" {
  value = aws_vpc.this.id
}

output "public_subnet_ids" {
  value = aws_subnet.public[*].id
}
```
Step 2: Call VPC Module in Root main.tf

Update main.tf:
```bash
# main.tf

provider "aws" {
  region = "us-east-1" # Change if needed
}

# 1. VPC
module "vpc" {
  source = "./modules/vpc"
}

# 2. ECR
module "ecr" {
  source          = "./modules/ecr"
  repository_name = "my-webapp-repo"
}

# 3. ECS
module "ecs" {
  source            = "./modules/ecs"
  cluster_name      = "my-webapp-cluster"
  ecr_repository_url = module.ecr.repository_url
  vpc_id            = module.vpc.vpc_id
  public_subnet_ids = module.vpc.public_subnet_ids
}
```

## Task 5: Deployment

1. Build the Docker image of your web app.

2. Push the Docker image to the Amazon ECR repository created by Terraform.

3. Run `terraform init` and `terraform apply` to deploy the ECS cluster and the web app

4. Access the web app through the public IP or DNS of the ECS service.

**Deploy:**
```bash
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```
![](12.%20terraform%20init.png)

![](13.%20terraform%20validate.png)

**Terraform Plan**
![](14.%20terraform%20plan.png)

![](15.%20terraform%20plan.png)

![](16.%20terraform%20plan.png)

![](17.%20terraform%20plan.png)

![](18.%20terraform%20plan.png)

![](19.%20terraform%20plan.png)

![](20.%20terraform%20plan.png)

**terraform apply -auto-approve:**

![](21.%20terraform%20apply.png)

Terraform will:
- Create VPC + Subnets
- Create Security Groups
- Create ECR repo
- Create ECS Cluster + Service + ALB + Target Group + Listener
- Wire ECR image into ECS


## Instructions:

1. Create a new directory for your Terraform project using a terminal (`mkdir terraform-ecs-webapp`).

2. Change into the project directory (`cd terraform-ecs-webapp`).

3. Create directories for the ECR and ECS modules (`mkdir -p modules/ecr and mkdir -p modules/ecs`).

4. Write the ECR module configuration (`nano modules/ecr/main.tf`) to create an ECR repository.

5. Write the ECS module configuration (`nano modules/ecs/main.tf`) to provision an ECS cluster and deploy the Dockerized web app.

6. Creat the main Terraform Configuration file (`nano main.tf) and use the ECR and ECS modules. 

```bash
module "ecr" {"\n  source = \"./modules/ecr\"\n  repository_name = \"your-webapp-repo\"\n"}

module "ecs" {"\n  source = \"./modules/ecs\"\n  ecr_repository_url = module.ecr.repository_url\n  // Add other variables as needed\n"}
```

7. Build the Docker image of your web app and push it to the ECR repository.

8. Run 'terraforn init' and "terrafore apply' to deploy the ECS cluster and the web app.

9. Access the web app through the public IP or DNS of the ECS service.

10. Document your observations and any challenges faced during the project.

## Side Note:
- Ensure you have the AWS CLI installed and configured with appropriate credentials.
- Modify variables and configurations in the modules based on your specific requirements.
- Replace placeholder values in the main configuration file with actual values.
- This is a learning exercise; use it to gain hands-on experience with Terraform, Docker, Amazon ECR, and ECS.

