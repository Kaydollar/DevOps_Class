# Mini Project- Working with Functions and Arrays in Shell-Scripting
This mini-project focuses on writing **modular and maintainable shell scripts** by using **functions**. The goal is to automate EC2 and S3 setup for a client of **DataWise Solutions**, while ensuring the script is well-organized and robust.

Key concepts implemented:

1. **Encapsulate logic in functions** to improve readability.
2. **Check script arguments** to guide proper usage.
3. **Validate AWS CLI installation** before executing AWS-related commands.
4. **Verify AWS profile environment variable** for authentication.

Each block of logic is implemented as a separate function and invoked in a clean, readable sequence. This structure reflects **real-world scripting practices.**

## Creating a Function in Shell-Scripting

Use the syntax: `function_name() {"\n    # Function body\n    # You can place any commands or logic here\n"}`

    Function name: This is the name of your function. Choose a descriptive name that reflects the purpose of the function.

    (): Parenthesis are used to define the function. They can be ommited in simpler cases, but its good practice to include them for clarity.

    {}: Curly braces encloses the body of the function, where you define the commands or logics that the function will execute.

**Task**

**Function: Checking if Function has Argument**

![](1.%20Check%20Arg.png).

**Function to activate infrastructure environment**

![](2.%20Infrastructure%20Environment.png)

**Function to check if AWS CLI is installed**

![](3.%20CLI%20install.png)

**Function to check if AWS profile is set**

![](4.%20AWS%20profile.png)

# === Execute all functions ===
check_num_of_args "$@"
activate_infra_environment
check_aws_cli
check_aws_profile

**Output**

![](5.%20Output.png)


