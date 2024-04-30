# Bash Scripting Cheat Sheet

![Bash Scripting Cheat Sheet](https://linuxconfig.org/wp-content/uploads/2022/03/00-bash-scripting-cheat-sheet.png)


Software Requirements and Linux Command Line Conventions

 Category | Requirements, Conventions or Software Version Used
---------- | ---------------------------
System | Any [Linux distro](https://linuxconfig.org/linux-download)
Software | Bash shell (installed by default)
Other | Privileged access to your Linux system as root or via the `sudo` command.
Conventions | **#** – requires given [linux commands](linux-commands) to be executed with root privileges either directly as a root user or by use of `sudo` command  <br> **$** – requires given [linux commands](linux-commands) to be executed as a regular non-privileged user


* * *


Bash Scripting Basics
---------------------

  
Here are some of the most basic things to know about Bash scripting. If you are not sure where to start, this would be a good choice.


 Syntax | Description
---------- | ---------------------------
`#!/bin/bash`  | Shebang that goes on the first line of every Bash script
`#!/usr/bin/env bash`  | Alternative (and better) shebang – using environment variable
`#`  | Used to make comments, text that comes after it will not be executed
`chmod +x script.sh && ./script.sh`  | Give script executable permissions and execute it
`$#`  | Stores the number of arguments passed to the Bash script
`$1, $2, $3`  | Variables that store the values passed as arguments to the Bash script
`exit`  | Exit from the Bash script, optionally add an error code
`Ctrl + C`  | Keyboard combination to stop Bash script in the middle of execution
`$( )`  | Execute a command inside of a subshell
`sleep`  | Pause for a specified number of seconds, minutes, hours, or days

Conditional statements
----------------------

Conditional statements with `if` or `case` allow for us to check if a certain condition is true or not. Depending on the answer, the script can proceed different ways.

 Syntax | Description
---------- | ---------------------------
`if then fi`  | Test a condition and execute the `then` clause if it is true
`if then else fi`  | Execute the `then` clause if the condition is true, otherwise execute the `else` clause
`if then elif else fi`  | Test multiple conditions and execute whichever clause is true


For `case` statements it is best to just see a basic example:

```bash
    #!/bin/bash
    
    day=$(date +"%a")
    
    case $day in 
    
      Mon | Tue | Wed | Thu | Fri)
        echo "today is a weekday"
        ;;
    
      Sat | Sun) 
        echo "today is the weekend"
        ;;
    
      *)
        echo "date not recognized"
        ;; 
    esac
```  

Basic `if` example script:

```bash
    #!/bin/bash
    
    if [ $1 -eq $2 ]; then
        echo "they are equal"
    else
        echo "they are NOT equal"
    fi
```  

Bash Loops
----------

Bash loops allow the script to continue executing a set of instructions as long as a condition continues to evaluate to true.

 Syntax | Description
---------- | ---------------------------
`for do done`  | Continue to loop for a predetermined number of lines, files, etc
`until do done`  | Continue to loop until a certain condition is met
`while do done`  | Continue to loop as long as a certain condition is true
`break`  | Exit the loop and continue to the next part of the Bash script
`continue`  | Exit the current iteration of the loop but continue to run the loop


Read User Input
---------------

Prompt the user for information to enter by using `read` command:

```bash
    #!/bin/bash
    
    read -p "What is your name? " name
    
    echo "Enjoy this tutorial, $name"
```  

Parse input given as arguments to the Bash script:
```bash
    #!/bin/bash
    
    if [ $# -ne 2 ]; then
    	echo "wrong number of arguments entered. please enter two."
    	exit 1
    fi
    
    echo You have entered $1 and $2.
``` 

* * *

Arithmetic Operators
--------------------



  
Arithmetic operators in Bash give us the ability to do things like addition, subtraction, multiplication, division, and other basic arithmetic inside of a Bash script.

 Syntax | Description
---------- | ---------------------------
`+`  | Addition
`-`  | Subtraction
`*`  | Multiplication
`/`  | Division
`%`  | Modulus
`**`  | Raise to a power
`((i++))`  | Increment a variable
`((i--))`  | Decrement a variable

Arithmetic Conditional Operators
--------------------------------

Arithmetic conditional operators are usually used on two numbers to determine if a certain condition is true or false.

 Syntax | Description
---------- | ---------------------------
` -lt `  | <
` -gt `  | \>
` -le `  | <=
` -ge `  | \>=
` -eq `  | \==
` -ne `  | !=

Note that the operators in the left column will work with single brackets `[ ]` or double brackets `[[ ]]`, whereas the operators in the right column will work only with double brackets.

String Comparison Operators
---------------------------

We can use string comparison operators to determine if a string is empty or not, and to check if a string is equal, less, or greater in length to another string.

 Syntax | Description
---------- | ---------------------------
`=` | equal
`!=` | not equal
`<` | less then
`>` | greater then
` -n s1 `  | string s1 is not empty
` -z s1 `  | string s1 is empty

Bash File Testing Operators
---------------------------

In Bash, we can test to see different characteristics about a file or directory.


 Syntax | Description
---------- | ---------------------------
` -b filename `  | Block special file
` -c filename `  | Special character file
` -d directoryname `  | Check for directory existence
` -e filename `  | Check for file existence
` -f filename `  | Check for regular file existence not a directory
` -G filename `  | Check if file exists and is owned by effective group ID.
` -g filename `  | true if file exists and is set-group-id.
` -k filename `  | Sticky bit
` -L filename `  | Symbolic link
` -O filename `  | True if file exists and is owned by the effective user id.
` -r filename `  | Check if file is a readable
` -S filename `  | Check if file is socket
` -s filename `  | Check if file is nonzero size
` -u filename `  | Check if file set-ser-id bit is set
` -w filename `  | Check if file is writable
` -x filename `  | Check if file is executable

* * *


Boolean Operators
-----------------

Boolean operators include and `&&`, or `||` and not equal to `!`. These operators allow us to test if two or more conditions are true or not.

 Syntax | Description
---------- | ---------------------------
`&&`  | Logical AND operator
`\|\|`  | Logical OR operator
`! `  | NOT equal to operator

Closing Thoughts
----------------

Feel free to reference this cheat sheet any time that you need a quick refresher. The goal here is to save you as much time as possible when trying to remember a certain Bash scripting component.
