# Creating AWS with Function and introducing Array (Mini-Project)

We'll begin this project by creating two functions:

1. One for provisioning EC2 instances and
2. Another for setting up S3 buckets

These functions will streamline the process of resource creation and enable us to automate tasks effectively.

## Function to provision EC2 instances

To programmatically create EC2 instances, you must use the [official documentation](https://docs.aws.amazon.com/cli/latest/reference/ec2/) to understand how to use ['aws cli' to create instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/) from the available commands, you will be able to interact with AWS programmatically.

![](1.%20Documentation.png)

If you search for for run-instance on the page with Control F on your keyboard, click on it and it will take you to the detailed documentation on the sub-command to create EC2 instances.

![](2.%20Instance%20run.png)

* Here is an example of how you would create EC2 instances using the command line.

```bash
    aws ec2 run-instances \
        --image-id "ami-0cd59ecaf368e5ccf" \
        --instance-type "t2.micro" \
        --count 5 \
        --key-name MyKeyPair \
        --region eu-west-2
```

**Note:** Make sure you have a key pair created in your aws console. Then you can replace the Mykeypair with your key pair name.

On the same page, if you search for one of the arguments, you will be able to read more about how to pass different arguments to the cli.

![](3.%20Example.png)

For the command to work: A keypair must already exist. You must create a keypair from the console. How to create a keypair

* Navigate to the aws EC2 aconsole
* Search for key pair at the left search bar
* Click on create

![](4.%20creating%20keypair.png)

* Input a name for your keypair and then create

![](5.%20keypair%20naming.png)

* keypair created successfully

![](6.%20Successful.png)

Now, let's update the shell script and create a function that will be responsible for creating EC2 instances.

```bash
#!/bin/bash

# Function to create EC2 instances
create_ec2_instances() {
    # Parameters
    instance_type="t2.micro"
    ami_id="ami-0cd59ecaf368e5ccf"  # Amazon Linux 2 in eu-west-2
    count=2
    region="eu-west-2"
    key_name="MyKeyPair"  # Make sure this key pair exists in the region

    echo "[INFO] Creating $count EC2 instance(s) in $region using AMI $ami_id"

    instance_ids=$(aws ec2 run-instances \
        --image-id "$ami_id" \
        --instance-type "$instance_type" \
        --count "$count" \
        --key-name "$key_name" \
        --region "$region" \
        --query "Instances[*].InstanceId" \
        --output text 2>/dev/null)

    if [ $? -eq 0 ] && [ -n "$instance_ids" ]; then
        echo "[SUCCESS] EC2 instances created: $instance_ids"
    else
        echo "[ERROR] Failed to create EC2 instances."
    fi
}

# Call the function
create_ec2_instances
```

$?: This is a special variable that holds the exit status of the last executed command. In this case, it checks if the aws ec2 run-instances command was successful. exit status that equals 0 is interpreted as successful. Therefore if exit code is "0", the echo the message to confirm that the previous command was successful.

We have once again used environment variables to hold the value of ami_id, count and region and replaced with their respective values with ami, count and region.

### Define function to create S3 buckets & learn about Arrays

Before diving into creating S3 buckets, it's beneficial to brush up on AWS S3 bucket concepts. If you need a quick refresher, consider watching [this informative video](https://www.youtube.com/watch?v=e6w9LwZJFIA)

The [AWS CLI reference for S3 can be found here](https://docs.aws.amazon.com/cli/latest/reference/s3api/). We will be using it in the script.

In this section, our objective is to create five distinct S3 buckets, each designated for storing data related to Marketing, Sales, HR, Operations, and Media.

To achieve this, we'll utilize a fundamental data structure in shell scripting known as an "array." this is because, we need one single variable holding all the data, and then have the capability to loop through them.

### Arrays in Shell Scripting

An array is a versatile data structure that allows you to store multiple values under a single variable name. Particularly in shell scripting, arrays offer an efficient means of managing collections of related data, making them invaluable for our task ahead.

Below is what the function would look like.

`create_S3_bucket` for different department.

``` bash
create_s3_buckets() {
    
    company="datawise" #Define a company name as prefix

    departments=("marketing" "sales" "hr" "operations" "media") #Array of department

    for department in "${departments[@]}"; do
        bucket_name="${company}-${department}-data-bucket-effiong"
    # Create S3 bucket using AWS CLI
        if ! aws s3api head-bucket --bucket "$bucket_name" --region "$region" 2>/dev/null; then
            aws s3api create-bucket --bucket "$bucket_name" --region $region
            if [ $? -eq 0 ]; then
                echo "Bucket '$bucket_name' created successfully in '$region'."
            else
                echo "Failed to create bucket '$bucket_name'."
            fi
        else
            echo "Bucket '$bucket_name' already exists."
        fi

    done
 
}
```

* Let's break down each part of the code

```bash
#!/bin/bash

# Function to create S3 buckets for different departments
create_s3_buckets() {

    # ---------------------------------------------
    # - Here, we define a variable named `company` and assign it the value "datawise".
    #   This variable will serve as the prefix for all S3 bucket names, ensuring their uniqueness.
    #   You should replace `datawise` with any other company name or unique identifier.
    #   This is because S3 buckets MUST be unique globally.
    # ---------------------------------------------
    company="datawise"

    # ---------------------------------------------
    # - This is where we define a variable that is an array.
    #   An array named `departments` is declared, containing the names of different departments.
    #   Each department name will be used to construct the name of an S3 bucket.
    # ---------------------------------------------
    departments=("Marketing" "Sales" "HR" "Operations" "Media")

    # ---------------------------------------------
    # - This line initiates a loop that iterates over each element in the `departments` array.
    #   For each iteration, the value of the current department name is stored in the variable `department`.
    # ---------------------------------------------
    for department in "${departments[@]}"; do

        # Convert department name to lowercase to conform to S3 naming rules
        dept_lower=$(echo "$department" | tr '[:upper:]' '[:lower:]')
        
        # Construct the bucket name
        bucket_name="${company}-${dept_lower}-data-bucket"

        echo "[INFO] Creating S3 bucket: $bucket_name"

        # Create the S3 bucket (assuming region is us-east-1; update if needed)
        aws s3api create-bucket --bucket "$bucket_name" --region "us-east-1"

        # Check if the bucket was created successfully
        if [ $? -eq 0 ]; then
            echo "[SUCCESS] S3 bucket '$bucket_name' created successfully."
        else
            echo "[ERROR] Failed to create S3 bucket '$bucket_name'. It may already exist or the name is not unique."
        fi

    done
}

# Call the function to execute
create_s3_buckets
```

``` bash
    The syntax ${departments[@]} in Bash refers to all elements in the array departments.

    [@]: This is an index or slice syntax specific to arrays in Bash. It signifies that we want to access all elements of the array.

    If you were interested in accessing a single element from the array, you would still use the syntax $departments[index]}, where index is the position of the element you want to access. Remember that array indexing in Bash starts from 0.
```

* An example is

``` bash
departments=("Marketing" "Sales" "HR" "Operations" "Media")

# Accessing the fourth element (Operations) from the array
echo "${departments[3]}"
```

* Output

`Operations`


Within the loop, we construct the name of the S3 bucket using the company prefix the current department name, and the suffix Data-bucket. This ensure that each bucket name is unique.

```bash
bucket_name="${company}-${department}-Data-Bucket"
```

Using the AWS CLI (aws s3api), we create an S3 bucket with the specified name from the variable ($bucket_name), in the specified AWS region (your-region). Make sure to replace "your-region" with the actual AWS region where you want to create the buckets.

``` bash
    # Create S3 bucket using AWS CLI
    aws s3api create-bucket --bucket "$bucket_name" --region your-region
```
This line checks the exit status of the previous command (aws s3api create-bucket). A value of 0 indicates success, while non-zero values indicate failure.

``` bash
if [ $? -eq 0 ]; then
```

Based on the exit status of the aws s3api command, we print a corresponding. message indicating whether the bucket creation was successful or not.

```bash
        echo "S3 bucket '$bucket_name' created successfully."
        else
            echo "Failed to create S3 bucket '$bucket_name'."
        fi
    done
```

This line marks the end of `create_S3_bucket` functions definition

### The complete script till this stage will now look like this

``` bash
#!/bin/bash

# Usage: ./script.sh <environment>
# Environment variable from argument
ENVIRONMENT=$1

# Function to check number of arguments
check_num_of_args() {
  if [ $# -ne 1 ]; then
    echo "Usage: $0 <environment>"
    exit 1
  fi
}

# Function to activate environment logic
activate_infra_environment() {
  if [ "$ENVIRONMENT" == "local" ]; then
    echo "Running script for Local Environment..."
  elif [ "$ENVIRONMENT" == "testing" ]; then
    echo "Running script for Testing Environment..."
  elif [ "$ENVIRONMENT" == "production" ]; then
    echo "Running script for Production Environment..."
  else
    echo "Invalid environment specified. Use 'local', 'testing', or 'production'."
    exit 2
  fi
}

# Function to check if AWS CLI is installed
check_aws_cli() {
  if ! command -v aws &> /dev/null; then
    echo "AWS CLI is not installed. Please install it before proceeding."
    exit 3
  fi
}

# Function to check if AWS profile is set
check_aws_profile() {
  if [ -z "$AWS_PROFILE" ]; then
    echo "AWS_PROFILE environment variable is not set."
    exit 4
  fi
}

# Function to create EC2 Instances
create_ec2_instances() {
  instance_type="t2.micro"
  ami_id="ami-0cd59ecaf368e5ccf"
  count=2
  region="eu-west-2"
  key_name="MyKeyPair"  # Make sure this key exists in your AWS region

  aws ec2 run-instances \
    --image-id "$ami_id" \
    --instance-type "$instance_type" \
    --count "$count" \
    --key-name "$key_name" \
    --region "$region"

  if [ $? -eq 0 ]; then
    echo "EC2 instances created successfully."
  else
    echo "Failed to create EC2 instances."
  fi
}

# Function to create S3 buckets for different departments
create_s3_buckets() {
  company="datawise"
  region="eu-west-2"
  departments=("Marketing" "Sales" "HR" "Operations" "Media")

  for department in "${departments[@]}"; do
    bucket_name="${company}-${department,,}-data-bucket"  # Lowercased name

    aws s3api create-bucket \
      --bucket "$bucket_name" \
      --region "$region" \
      --create-bucket-configuration LocationConstraint="$region"

    if [ $? -eq 0 ]; then
      echo "S3 bucket '$bucket_name' created successfully."
    else
      echo "Failed to create S3 bucket '$bucket_name'."
    fi
  done
}

# === Execute the script steps ===
check_num_of_args "$@"
activate_infra_environment
check_aws_cli
check_aws_profile
create_ec2_instances
create_s3_buckets
```

