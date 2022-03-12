# Shell-scripting-traversy

#! /usr/bin/bash

<br>
<br>
<br>

### first line of .sh file must be result of which bash
### Space will interupt code exection 
### But Space must be given where it is needed like - [ ]
### Bash is case sensitive like other UNIX
### fi is used to end conditional statement

<br>
<br>
<br>
<br>
<br>

#############
# ECHO Command

`echo "Hello World!"`

<br>
<br>
<br>
<br>
<br>

#############
# VARIABLE

## UpperCase by Convention
## Letters, Numbers, Under_Score is allowed
## Put a $ before variable to print it in string

`NAME="Jubyer"`

`echo Hello I am $NAME`

## ${var_Name} this is also allowed

`echo "Hello I am ${NAME}" `

<br>
<br>
<br>
<br>
<br>

#############
# USER INPUT 

## command command_execution_style Message_to_show variable_to_store_input

`read -p "Enter your name: " NAME`

`echo "Hello ${NAME}, nice to meet you"`

<br>
<br>
<br>
<br>
<br>

#############

# Conditional Statement

## IF statement
`if [ "$NAME"=="Shezan" ]`

`then`

`  echo "Hello I am $NAME"`

`fi`

## IF-ELSE statement

`if [ "$NAME" == "Shezan" ]`

`then`

`  echo "Hello I am $NAME"`

`else`

`  echo "Your Name is not Shezan, you're $NAME"`

`fi`

## ELSE-IF (elif) statement

`if [ "$NAME" == "Shezan" ]`

`then`

`  echo "Hello I am $NAME"`

`elif [ "$NAME" == "Jack" ]`

`then`

`  echo "Oh!! so you're $NAME"`

`else`

`  echo "Your Name is not Shezan, you're $NAME"`

`fi`

<br>
<br>
<br>
<br>
<br>

#############
# COMPARISIONAL


## val1 -eq val2 --> Return true if values are equal
## val1 -nq val2 --> Return true if values are not equal
## val1 -gt val2 --> Return true if val1 is greater than val2
## val1 -ge val2 --> Return true if val1 is greater than or equal val2
## val1 -lt val2 --> Return true if val1 is less than val2
## val1 -le val2 --> Return true if val1 is less than or equal val2
#############


`VAL1=9`

`VAL2=9`

`if [ "$VAL1" -gt "$VAL2" ]`

`then`

`  echo "$VAL1 is greater than $VAL2"`

`elif [ "$VAL1" -lt "$VAL2" ]`

`then `

`  echo "$VAL1 is less than $VAL2"`


`else `

`  echo "$VAL1 is equal $VAL2"`

`fi `

<br>
<br>
<br>
<br>
<br>

#############

# WORKING WITH FILE 

## -d file  --> True if the file is a directory or folder
## -e file  --> True if the file exists (note that this is not particularly portable, thus -f is generally used )
## -f file  --> True if the provided string is a file
## -g file  --> True if the group is set on a file
## -r file  --> True if the file is readable
## -s file  --> True if the file has a non-zero size
## -u  --> True if the user id is set on a file
## -w  --> True if the file is writable
## -x  --> True if the file is executable

<br>
#############

<br>
<br>
<br>

`FILE="test.txt"`

`if [ -d "$FILE" ]`

`then`

`  echo "$FILE is a file"`

`else`

`  echo "$FILE is not a file"`

`fi`

<br>
<br>
<br>
<br>
<br>

#############
# SWITCH statement

`read -p "Are you over 18 Y/N: " ANS`

`case "$ANS" in `

`  [yY] | [yY][eE][sS])`

`    echo "okay You can Vote"`

`    ;; # this work as break`

`  [nN] | [nN][oO])`

`    echo "No you can't Vote"`

`    ;;`

`  *) # this work as default`

`    echo "Please enter y/yes or n/no"`

`    ;;`

`esac # indicates end of switch spelled as "case" in backward`

<br>
<br>
<br>

#############

<br>
<br>
<br>
<br>
<br>


#############

# FOR LOOP

## SIMPLE FOR LOOP

`NAMES="Saikat Tanzid Rasel Shezan"`

`for NAME in $NAMES # like javascript`

`  do # defining what to do`

`    echo "Hello $NAME" # statement`

`done # ending loop`

<br>
<br>
<br>

## printing number

`N="1 2 3 4"`

`for I in $N `

`  do `

`  echo $I`

`done `

<br>
<br>
<br>

## Renaming file with FOR LOOP
`FILES=$(ls *.txt) # saving file name in a variable`

`NEW="new" # setting new prefix for fileName`

`for F in $FILES`

`  do `

`    echo "Renaming file from $F to new-$F"`

`    mv $F $NEW-$F # moving file so that old file moved with newName`

`done `

<br>
<br>
<br>
<br>
<br>

#############
# WHILE LOOP

`LINE=1 # indicating line Number `


`while read -r CURRENT_LINE # Reading from file till end`

`  do `

`    echo "$LINE: $CURRENT_LINE" # printing line by line`

`    ((LINE++)) # increamenting line number`

`done < "./new-$.txt" # mentioning file path`

<br>
<br>
<br>
<br>
<br>

#############
# FUNCTION - its more like jaavscript/PHP function

`function sayHello(){`

`  echo "Hello World!!"`

`}`

`sayHello`

<br>
<br>
<br>

#############
## FUNCTION with PARAMETERS
## we dont have pass variable in ()
## just mentioning $1, $2 will work as first parameter, 2nd param

`function greet(){`

`    echo "Hello $1, also Hello $2"`

`}`

`greet "Jubyer" "Shezan"`

<br>
<br>
<br>
<br>
<br>

#############
## CREATE A FOLDER AND WRITE A FILE IN IT

`mkdir hello `

`touch "hello/world.txt"`

`echo "Hello World" >> "hello/world.txt"`

`echo "Created \"hello/world.txt\" "`

<br>
<br>
<br>
<br>
<br>

#############
# ITERATING VALUES FROM FILE 

`PATH="/hello/world" `

`for VALUE in $( cat $PATH )`

`  do` 

`    echo "Characters of Hera Pheri: $VALUE"`

`done `
