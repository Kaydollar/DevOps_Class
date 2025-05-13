# Control Flow In Shell Scripting
    Control flow in shell scripting dictates the order in which commands are executed. By default, a shell script executes commands sequentially from top to bottom. However, control flow statements enable the script to make decisions, repeat actions, or alter the execution path based on conditions or events.

## Types of Control Flow Statements
### Conditional Statements:
    `if, elif, else: These statements allow the script to execute different blocks of code based on whether a condition is true or false.

    case: This statement provides a way to execute different commands based on the value of a variable, allowing for multi-way branching.

### Looping Statements:
    for: This loop iterates over a list of items, executing a block of code for each item. It's useful for processing multiple files, lines in a file, or a sequence of numbers.

    while: This loop executes a block of code repeatedly as long as a condition is true. It's suitable for situations where the number of iterations is not known beforehand.
    
    until: This loop executes a block of code repeatedly until a condition becomes true.

# TASK
**Vim file Created named `control_flow.sh`**

![](1.%20Vim%20shell.png)

**Putting a code in the *control_flow.sh* `<read -p "Enter a number: " num` 
 and adding a shebang `#! /bin/bash` showing its should be run with a Bash Interpreter. 

 ![](2.%20First%20code.png)

**Executing the Script** 
    
When Executed, the Script Just ask to Enter a number: When the number was typed and hit enter, it takes the number, but won't visibly see what it does to the number. This is because the **read** command has a way of taking input from the user, and storing the value into a variable passed to the **read** command
    
The **read** command is used to capture user input and store it in a variable

![](./3.%20Execute%20first%20code.png)

## if statement
The `if` statement in Bash allows you to execute commands based on conditions. using the command:

    if [condition]; then
        commands
    fi

    if: This keyword starts the conditional statement.

    [condition]: The condition to evaluate Bracket[] are used to enclose the condition being tested.

    then: if condition is true, execute the commannds that follow this keyword.

    fi: Ends the if statement. It's bascically if spelled backward, indicating the conclusion of the conditional block.

if the value in $num is greater than 0, its shows **The number is Positive**
![](4.%20Execute%20code%202.png)

![](5.%20Output.png)

## elif statement

    elif stands for else if, allowing a test of additional conditions, if the previous condition were not met. It helps in adding more layers of decision-making to our scripts. 
    
The syntax for using elif:

    if [ condition1 ]; then 
    commands1
    elif [ condition2 ]; then 
    commands2
    fi

elif: The keyword is used after an if or another elif block. it allows to specify an alternative condition to test if the previous condition is false.

[condition 2]: The new condition you want to evaluate. Like the if statement, this is enclosed in square brackets[]

then: if the elif condition is true, execute the commands that follow the keyword

    #!/bin/bash

    read -p "Enter a number: " num
    echo "You have entered the number $num"
    if [ $num -gt 0 ]; then
    echo "The number is positive."
    elif [ $num -lt 0 ]; then
    echo "The number is negative."
    fi

![](6.%20Execute%20code%203.png)

The syntax for using else:

    if [ condition1 ]; then 
    commands1
    elif [ condition2 ]; then 
    commands2
    else [initial value]
    fi

elif: The keyword is used after an if or another elif block. it allows to specify an alternative condition to test if the previous condition is false.

[condition 2]: The new condition you want to evaluate. Like the if statement, this is enclosed in square brackets[]

then: if the elif condition is true, execute the commands that follow the keyword

then[Condition 3]: if the two conditions [command 1] and [command 2] isn't true return [default value]

#!/bin/bash

read -p "Enter a number: " num

if [ $num -gt 0 ]; then
    echo "The number is positive."
elif [ $num -lt 0 ]; then
    echo "The number is negative."
else
    echo "The number is zero."
fi

![](15.%20Execute%20code%203.png)

![](16.%20Output.png)

# Looping Statements | Shell Script
Looping Statements in Shell Scripting: There are total 3 looping statements that can be used in bash programming.

`while` statement in Shell Script in Linux

`for` statement in Shell Script in Linux

`until` statement in Shell Script in Linux

To alter the flow of loop statements, two commands are used they are,  

1. break
2. continue

## For Loop
The for loop: for each of items in a given list, perform the given set of commands. It has the following syntax.

    for var in <list>
    do
    <commands>
    done

The for loop will take each item in the list (in order, one after the other), assign that item as the value of the variable var, execute the commands between do and done then go back to the top, grab the next item in the list and repeat over.

The list is defined as a series of strings, separated by spaces.

The for loop has two main form
1. list form: iterates over a list of items

Syntax:
    
    for item in item1 item2 item3; do
    echo $item
    done

2. C-style Form

    This style allows you to specify an initializer, condition and increment/decrement expression. Its based on the syntax used in doing a **for** loop in C programming.

Syntax:

    for (( i=0; i<5; i++ )); do
    echo "Number $i"
    done

- "for (( ... ));" : This is the syntax that starts a C-style for loop in Bash. It's distinguished from the list form by the double parentheses "(( ... ))", which enclose the three parts of the loop: "initialization, condition, and increment/decrement".

- "i=0": This is the initialization part. Before the loop starts, "i" is set to "0". This typically sets up a counter variable to start from a certain value. In this case, i starts from 0.

- "i<5": This is the condition for the loop to continue running. After each iteration of the loop, Bash checks this condition. If it's true, the loop continues; if it's false, the loop ends. Here, the loop will continue as long as **i** is less than "5".

- "i++": This is the increment expression. It's executed at the end of each loop iteration. i++ is shorthand for incrementing i by 1 (i = i + 1). This step ensures that the loop will eventually end by changing the value of i so that the condition i<5 will not always be true.

- "do ... done": Encloses the commands to be executed in each iteration of the loop. Here, the command inside the loop is **echo "Number $i"**, which prints the current value of "i" to the console.

## How it works

- Initialization: Before the first iteration, "i" is set to "0".
- Condition Check: Before each iteration, including the first, Bash checks if i is less than 5.
    
    - If the condition is true, Bash executes the commands inside the loop.
   
    - If the condition is false, Bash exits the loop.

- Execute Commands: The command(s) inside the "do ... done" block are executed. In this case, it prints the current value of "i".

- "Increment:" After executing the commands, "i" is incremented by "1" using the syntax "(i++)".

- **Repeat:** Steps 2 through 4 are repeated until the condition in step 2 is false.

**Lets take a Walkthrough to further expand on your understanding**

- "First Iteration:" i=0, condition 0<5 is true, prints "Number 0", increments i to 1.

- "Second Iteration:" i=1, condition 1<5 is true, prints "Number 1", increments i to 2.

- "Continues iteration" ...

- "Fifth Iteration:" i=4, condition 4<5 is true, prints "Number 4", increments i to 5.

- "Sixth Check:" i=5, condition 5<5 is false, loop ends.

## Task 1
- Create a List-Form shell script named `for_list-form.sh`

![](7.%20vim%20for_list-form.png)

![](8.%20code.png)

Line 4 - Create a simple list which is a series of names.

Line 6 - For each of the value in the list $values assign the values to the variable $value and do the following commands.

Line 8 - echo the value to the screen just to show that the mechanism works. We can have as many commands here as we like.

Line 11 - echo another command to show that the bash script continued execution as normal after all the items in the list were processed.

- Permission update syntax:
    
        sudo chmod +u for_list-form.sh

![](9.%20Permission.png)

- Terminal Output:

![](10.%20Terminal%20Output.png)

## Task 2

- Create a List-Form shell script named `for_C-style_form.sh`

Syntax: `vim for_C-style_form.sh`

![](12.%20vim%20for_C-style_form.png)

![](11.%20code2.png)

- Permission update syntax

        sudo chmod +x for_C-style_form.sh

![](13.%20Permission%202.png)

- Terminal Output

![](14.%20Terminal%20Output.png).







