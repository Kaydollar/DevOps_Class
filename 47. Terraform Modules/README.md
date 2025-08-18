# Mini Project: Terraform Module - VPC and S3 Bucket with Back End.

**Purpose:**

In this mini project, you will use Terraform to create modularized configurations for building an Amazon Virtual Private Cloud (VPC) and an Amazon S3 bucket. Additionally, you will configure Terraform to use Amazon S3 as the backend storage for storing the Terraform state file.

**Objectives:**

1. **Terraform Modules:**
- Learn how to create and use Terraform modules for modular infrastructure provisioning.

2. **VPC Creation:**
- Build a reusable Terraform module for creating a VPC with specified configurations.

3. **S3 Bucket Creation:**
- Develop a Terraform module for creating an S3 bucket with customizable settings.

4. **Backend Storace Confauration:**
- Configure Terraform to use Amazon S3 as the backend storage for storing the Terraform state file.

**Project Tasks:**
## Task 1: VPC Module
1. Create a new directory for your Terraform project (e.g., `terraform-modules-vpc-s3`).
![](1.%20mkdir.png)

2. Inside the project directory, create a directory for the VPC module (e.g., `modules/vpc`).
![](2.%20Modules.png)

3. Write a Terraform module (`modules/vpc/main.tf`) for creating a VPC with customizable configurations such as CIDR b subnets, etc.
![](3.%20vpc.png)

modules/vpc/main.tf
```bash
resource "aws_vpc" "this" {
  cidr_block           = var.vpc_cidr
  enable_dns_support   = true
  enable_dns_hostnames = true
  tags = merge(
    var.tags,
    { Name = "${var.name}-vpc" }
  )
}

resource "aws_subnet" "public" {
  count                   = length(var.public_subnets)
  vpc_id                  = aws_vpc.this.id
  cidr_block              = var.public_subnets[count.index]
  availability_zone       = var.azs[count.index]
  map_public_ip_on_launch = true
  tags = merge(
    var.tags,
    { Name = "${var.name}-public-${count.index + 1}" }
  )
}

resource "aws_internet_gateway" "this" {
  vpc_id = aws_vpc.this.id
  tags   = merge(var.tags, { Name = "${var.name}-igw" })
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.this.id
  tags   = merge(var.tags, { Name = "${var.name}-public-rt" })
}

resource "aws_route" "public_internet_access" {
  route_table_id         = aws_route_table.public.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.this.id
}

resource "aws_route_table_association" "public" {
  count          = length(var.public_subnets)
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}
```
modules/vpc/variables.tf
```bash
variable "tags" {
  type    = map(string)
  default = {}
}
```
modules/vpc/outputs.tf
```bash
output "vpc_id" { value = aws_vpc.this.id }
output "public_subnet_ids" { value = aws_subnet.public[*].id }
```

4. Create a main Terraform configuration file (`main.tf`) in the project directory, and use the VPC module to create a VP
![](4.%20main.tf.png)

### This module will:
- Create a VPC
- Create public subnets
- Create internet gateway
- Create route table + routes
- Associate subnets to route table

## Task 2: S3 Bucket Module
1. Inside the project directory, create a directory for the S3 bucket module (e.g., `modules/s3`).

2. Write a Terraform module (`modules/s3/main.tf`) for creating an S3 bucket with customizable configurations such as bucket name, ACL, etc.

![](5.%20S3.png)

![](6.%20touch.png)

modules/s3/main.tf

```bash
resource "aws_s3_bucket" "this" {
  bucket = var.bucket_name
  tags   = var.tags
}

resource "aws_s3_bucket_acl" "this" {
  bucket = aws_s3_bucket.this.id
  acl    = var.acl
}

resource "aws_s3_bucket_versioning" "this" {
  bucket = aws_s3_bucket.this.id
  versioning_configuration {
    status = var.versioning_enabled ? "Enabled" : "Suspended"
  }
}
```
modules/s3/variables.tf
```bash
variable "acl" {
  type    = string
  default = "private"
}
```
modules/s3/outputs.tf
```bash
output "bucket_name" { value = aws_s3_bucket.this.bucket }
output "bucket_arn" { value = aws_s3_bucket.this.arn }
```

3. Modify the main Terraform configuration file (`main.tf`) to use the S3 module and create an S3 bucket.
![](7.%20s3%20main.tf.png)

### This module will:
- Create an S3 bucket
- Apply ACL (private, public-read, etc.)
- Enable versioning if needed

## Task 3: Backend Storage Configuration

1. Configure Terraform to use Amazon S3 as the backend storage for storing the Terraform state fle.

2. Create a backend configuration file (e.g., `backend.tf`) specifying the S3 bucket and key for storing the state.
![](8.%20touch%20backend%20&&%20main.png)

backend.tf
```bash
terraform {
  backend "s3" {
    bucket = "my-terraform-state-bucket" # Must already exist
    key    = "vpc-s3/terraform.tfstate"
    region = "us-east-1"
  }
}
```
Main Configuration – Root main.tf
```bash
provider "aws" {
  region = "us-east-1"
}

module "vpc" {
  source          = "./modules/vpc"
  name            = "myproject"
  vpc_cidr        = "10.0.0.0/16"
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24"]
  azs             = ["us-east-1a", "us-east-1b"]
  tags = {
    Environment = "Dev"
    Project     = "TerraformVPC"
  }
}

module "s3" {
  source            = "./modules/s3"
  bucket_name       = "myproject-terraform-bucket-1234"
  acl               = "private"
  versioning_enabled = true
  tags = {
    Environment = "Dev"
    Project     = "TerraformS3"
  }
}

output "vpc_id" {
  value = module.vpc.vpc_id
}

output "public_subnet_ids" {
  value = module.vpc.public_subnet_ids
}

output "s3_bucket_name" {
  value = module.s3.bucket_name
}
```

### Setup Backend Storage

Terraform needs a place to store its state file (instead of your local machine).
That’s why we configure S3 backend.

👉 Important: The S3 bucket you use in backend.tf must already exist before terraform init.

So, create it once:
```bash
terraform {
  backend "s3" {
    bucket = "my-terraform-state-backend-project-darey" # Must already exist
    key    = "vpc-s3/terraform.tfstate"
    region = "us-east-1"
  }
}
```

3. Initialize the Terraform project using the command: `terraform init`.

![](9.%20terraform%20init.png)

4. Check what Terraform will create:
`terraform plan`

![](10.%20terraform%20plan.png)

![](11.%20terraform%20plan.png)

![](12.%20terraform%20plan.png)

![](13.%20terraform%20plan.png)

5. Apply the Terraform configuration to create the VPC and S3 bucket using the command: `terrafore apply`.

![](14.%20terraform%20apply.png)

![](15.%20S3%20confirmation.png)

![](16.%20VPC%20confirmation.png)

Terraform destroy

![](17.%20Terraform%20destroy.png)

![](18.%20Terraform%20destroy.png)

![](19.%20terraform%20destroy.png)

![](20.%20terraform%20destroy.png)

![](21.%20Terraform%20destroy.png)
