# Error Handling in Shell Scripting
Shell scripting is a powerful tool for automating tasks in Unix-based operating systems. However, as with any programming language, handling errors effectively is crucial for ensuring the robustness and reliability of scripts. Without proper error handling, a script might continue to execute even when an error has occurred leading to unexpected result or even system crashes. In Bash Script development, Exiting when an error occurs is an important aspect, as it helps to ensure that script stops running and exits cleanly in an event of error. Also it is important to include proper error messages in script to provide useful information to user if an error occurs.

## Checking exit status code:

Every command in Unix returns returns an exit status code upon completion. zero exit status code indicates success, while non-zero codes indicate failure. exit status of a command can be captured using the $? variable immediately after executing the command.

![Exit_Status](./1.%20Exit_Status.png)

### Output:
![](2.%20Output.png)

## Using if Statements for Error Checking

Modify the script from `exit status code` to include more commands (e.g., creating a file inside the directory) and use if statements to handle errors at each step.

![](3.%20Else.png)

### Output:

![](4.%20Output.png)

## Creating Custom Error Messages
Logging errors can be immensely helpful for troubleshooting. Redirecting error messages to a log file can provide valuable insights into what went wrong during script execution. For example, the following script will print error message in a file named error.log not on console.

![error_message](5.%20Error%20Message.png)

### Output

![](6.%20Output.png)

## Using trap command.

The trap command allows to execute a command or set of commands when a specific situation occurs. Here I will talk about ERR only as we are discussing error handling. For example, the following script uses the trap command to capture line number and script name if the command executes with a non-zero exit code .

![Trap](7.%20Trap.png)

### Output

![](8.%20Output.png)

## Using set -e:

The set -e option in shell scripts instructs the shell to immediately exit if any command exits with a non-zero status. This ensures that your script stops executing as soon as an error occurs.

![set -e](9.%20set-e.png)

### Output

![](10.%20Output.png)

 ## Conclusion
Effective error handling is essential for writing robust and reliable shell scripts. By implementing techniques like checking exit status codes, logging errors, using trap commands and set -e, you can ensure that your scripts handle errors gracefully and continue to perform optimally even in challenging environments. Remember to test thoroughly and continuously improve error handling to enhance the resilience of your shell scripts.
