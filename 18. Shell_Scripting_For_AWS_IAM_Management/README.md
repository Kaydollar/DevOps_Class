# Shell Scripting for AWS IAM management (Capstone Project)

## Project Scenario

CloudOps Solutions is a growing company that recently adopted AWS to manage its cloud infrastructure. As the company scales, they have decided to automate the process of managing AWS Identity and Access Management (IAM) resources. This includes the creation of users, user groups, and the assignment of permissions for new hires, especially for their DevOps team.

## Purpose

You will extend your shell scripting capabilities by creating more functions inside the "aws-iam-manager.sh" script to fulfill the objectives below. Ensure that you have already configured AWS CLI in your terminal and the configured AWS Account have the appropriate permissions to manage IAM resources.

## Objectives:

Extend the provided script to include IAM management by:

1. Defining IAM User Names Array to store the names of the five IAM users in an array for easy iteration during user creation.

2. Create the IAM Users as you iterate through the array using AWS CLI commands

3. Define and call a function to create an IAM group named "admin" using the AWS CLI commands.

4. Attach an AWS-managed administrative policy (e.g., "AdministratorAccess") to the "admin" group to grant administrative privileges.

5. Iterate through the array of IAM user names and assign each user to the "admin" group using AWS CLI commands.

``` bash
#!/bin/bash

PROFILE="cassie"

# Define IAM User Names Array
IAM_USER_NAMES=("alice" "bob" "charlie" "diana" "eric")  # Replace with actual usernames

create_iam_users() {
    echo "Creating IAM users..."
    for USER in "${IAM_USER_NAMES[@]}"; do
        if aws iam get-user --user-name "$USER" --profile "$PROFILE" >/dev/null 2>&1; then
            echo "User $USER already exists."
        else
            aws iam create-user --user-name "$USER" --profile "$PROFILE" && echo "Created user: $USER" || echo "Failed to create user: $USER"
        fi
    done
    echo ""
}

create_cloudops_admin_group() {
    echo "Setting up 'cloudOps-admin' group..."
    if ! aws iam get-group --group-name "cloudOps-admin" --profile "$PROFILE" >/dev/null 2>&1; then
        aws iam create-group --group-name "cloudOps-admin" --profile "$PROFILE" && echo "Group created: cloudOps-admin"
    else
        echo "Group already exists: cloudOps-admin"
    fi

    aws iam attach-group-policy \
        --group-name "cloudOps-admin" \
        --policy-arn arn:aws:iam::aws:policy/AdministratorAccess \
        --profile "$PROFILE" \
        && echo "Policy attached to group." || echo "Failed to attach policy."

    echo ""
}

add_users_to_cloudops_admin_group() {
    echo "Adding users to 'cloudOps-admin' group..."
    for USER in "${IAM_USER_NAMES[@]}"; do
        aws iam add-user-to-group \
            --user-name "$USER" \
            --group-name "cloudOps-admin" \
            --profile "$PROFILE" \
            && echo "Added $USER to group." || echo "Failed to add $USER."
    done
    echo ""
}

main() {
    if ! command -v aws &> /dev/null; then
        echo "AWS CLI not installed or not in PATH."
        exit 1
    fi

    create_iam_users
    create_cloudops_admin_group
    add_users_to_cloudops_admin_group

    echo "IAM setup complete."
}

main
exit 0
```

## Project Deliverable

* AWS Configuration

![](1.%20Aws%20Configure.png)

* Users created from the script output

![](2.%20Usere%20created.png)

* CloudOps-admin group created

![](3.%20Group%20create.png)

* All five users added to CloudOps group

![](4.%20Users%20added%20to%20group.png)

* CloupOps group created on AWS IAM console

![](5.%20CloudOps%20group%20created.png)

* All the five user created in the CloudOpd-admin group

![](6.%20User%20added%20to%20group.png)

* Administrative access policy attached to CloudOps-admin group

![](7.%20Administrative%20Policy%20added.png)

## AWS IAM Management Script Overview

This script provides automated management of AWS IAM resources with the following features:

### Script Functionality

Defines an array of IAM users to be created

Provides functions to:

* Create IAM users
* Create an admin group
* Attach the AdministratorAccess policy to the group
* Add all users to the admin group Includes error checking for each AWS CLI operation
* Implements a main function that orchestrates the execution flow.