# Mini Project - 5 Essential Skills to Elevate your Shell Scripting Journey into Cloud Computing.

As we emback on the next phase of your journey into tech, getting your feet strong in shell scripting is one of the most important, and foundational skills you must not miss. The skills and concepts you've mastered so far have laid the groundwork for what comes next. However, the journey ahead is not just a continuation-it's a leap towards achieveing great success in the realms of Cloud, DevOps, Systm Engineering, and related fields. The areas we are about to explore are not merely topics; they are the keys to unlocking profound capabilities in automation, problem-solving, and innovation in the digital world.

## Objective

Our objectinve for the next series of projects is to advance your shell scripting skills while also integrating and utilizing cloud services. By focusing on the following 5 critical concepts, we aim to elevate your capabilities to a level where you can confidently navigate. By focusing on the following 5 critical concepts, we aim to elevate your capabilities to a level where you can confidently navigate and harness the power of both shell scripting and cloud computing. This combination is not just about learning commands and servies; it's about understanding how to leverage these tools in unison to build, deploy, and manage scalable, efficient and robust systems in the cloud.

1. Functions

2. Arrays

3. Environment Variables

4. Command Line Arguments

5. Error Handling

## Background:

DataWise Solutions is a forward-thinking Data Science Consulting Company that specializes in deploying analytical and machine learning environments to support data-driven decision-making for its clients. Recongnizing the need for agility and efficiency in setting up such environments, DataWise has decided to streamline its deployment process on AWS, focusing on simplicity and automation.

## Scenario:

One of DataWise Solutions' clients, a burgeoning e-commerce startup, is looking to harness the power of data science to analyze customer behavior and enhance their shopping experience. The startup wishes to deploy their data science workspace on AWS, utilizing EC2 instances for computational tasks and S3 buckets for storing their vast datasets of customer interactions.

## Specific Requirements:

To meet the client's needs and ensure a practical learning experier, Data Wise Solutions plans to develop a script that automates the setup of EC2 stances and S3 buckets. This script will incorporate the 5 critical shell scripting co cepts previously identified:

1. Functions: Implement custom functions to modularize tasks like creating EC2 instances, configuring S3 buckets, and verifying deployment statu

2. Arrays: Use arrays to manage a list of resources created, ensuring isy tracking and manipulation.

3. Environment Variables: Leverage environment variables to store sensitive information like AWS credentials, region settings, and configuration parameters, enhancing script portability and security.

4. Command Line Arguments: Accept command line arguments to customize the script's behavior, such as specifying the EC2 instance type or S3 bucket name, allowing for dynamic and flexible deployments.

5. Error Handling: Implement robust error handling mechanisms to catch and respond to AWS service exceptions, ensuring the script can recover gracefully from failures.

```bash
#!/bin/bash

# Load environment variables (you can set these in a .env file and source it)
AWS_REGION=${AWS_REGION:-"us-east-1"}
AMI_ID=${AMI_ID:-"ami-0c02fb55956c7d316"}  # Amazon Linux 2
INSTANCE_TYPE=${INSTANCE_TYPE:-"t2.micro"}

# Hardcoded resource names (S3 bucket must be unique globally)
EC2_INSTANCE_NAME="Datawise-solution-Ec2"
S3_BUCKET_NAME="datawisesolution-sandradevops-s3-$(date +%s)"

# Function to print error and exit
error_exit() {
    echo "[ERROR] $1"
    exit 1
}

# ✅ FIXED Function to create S3 bucket
create_s3_bucket() {
    echo "[INFO] Creating S3 bucket: $S3_BUCKET_NAME in region $AWS_REGION"

    if [ "$AWS_REGION" = "us-east-1" ]; then
        aws s3api create-bucket \
            --bucket "$S3_BUCKET_NAME" \
            --region "$AWS_REGION"
    else
        aws s3api create-bucket \
            --bucket "$S3_BUCKET_NAME" \
            --region "$AWS_REGION" \
            --create-bucket-configuration LocationConstraint="$AWS_REGION"
    fi

    if [ $? -ne 0 ]; then
        error_exit "Failed to create S3 bucket: $S3_BUCKET_NAME. It may already exist or the name is not unique."
    fi

    echo "[SUCCESS] S3 bucket created: $S3_BUCKET_NAME"
}

# Function to create EC2 instance
create_ec2_instance() {
    echo "[INFO] Launching EC2 instance: $EC2_INSTANCE_NAME"

    instance_params=(
        --image-id "$AMI_ID"
        --count 1
        --instance-type "$INSTANCE_TYPE"
        --region "$AWS_REGION"
        --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value=$EC2_INSTANCE_NAME}]"
    )

    instance_id=$(aws ec2 run-instances "${instance_params[@]}" \
        --query 'Instances[0].InstanceId' --output text 2>/dev/null)

    if [ -z "$instance_id" ]; then
        error_exit "Failed to launch EC2 instance"
    fi

    echo "[SUCCESS] EC2 Instance launched: $instance_id"
}

# Main function
main() {
    echo "[START] AWS Resource Setup Script"
    create_s3_bucket
    create_ec2_instance
    echo "[DONE] All resources created successfully"
}

# Run the main function
main
```

### Project Execution

* Bash script showing the creation of EC2 and S3 bucket function

![](1.%20Vim.png)

* The datawise solution file ran to show the execution of the successfull creation of Ec2 and S3 bucket

![](2.%20Success.png)

* List command to show our bash script file executed

![](3.%20set.png)

* Ec2 instance successfully created and launched on our Ec2 server

![](4.%20Datawise-solution%20via%20AWSCLI.png)

* S3 bucket successfully created created on the S3 dashboard

![](5.%20S3%20created%20via%20AWSCLI.png)



