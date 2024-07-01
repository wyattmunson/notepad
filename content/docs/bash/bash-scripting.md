---
title: "Bash Scripting"
description: ""
lead: ""
date: 2023-06-26T17:35:47-07:00
lastmod: 2023-06-26T17:35:47-07:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "bash-scripting-73191668904528a482e44afaddf33e5c"
weight: 999
toc: true
---

## Variables

```bash
# set a variable
VARIABLE_NAME=some_text

# reference a variable
echo $VARIABLE_NAME

# set variable equal to a command output
VARIABLE_NAME=$(echo "Hello")

# set default variable
VARIABLE_NAME="${1:-HELLO}"
```

### Bash Builtin Variables

```bash { title="Builtin bash variables" }
$0        # Name of the Bash script
$1 - $9   # First 9 arguments to the script
$#        # Number of arguments were passed to script
$@        # All the arguments supplied to the Bash script.
$?        # The exit status of the most recently run process.
$$        # The process ID of the current script.
$_        # Last argument of the previous command
$USER     # The username of the user running the script.
$HOSTNAME # The hostname of the machine the script is running on.
$SECONDS  # The number of seconds since the script was started.
$RANDOM   # Returns a different random number each time is it referred to.
$LINENO   # Returns the current line number in the Bash script.
```

### Check if Variable Exists

Bash scripts that expect environment variables can be enriched by checking to see if the variables exist before running the remainder of the script. Help text can be provided and an exit code can be thrown if the conditions are not met.

```bash
#check if variable exists
if [ -n "$1" ]; then
  echo "You supplied the first parameter!"
else
  echo "First parameter not supplied. Exiting."
  exit 1
fi

# variable does not exist
if [ -z "$1" ]; then
  echo "Does not exists"
fi

# check if exists, otherwise set
if [ -z ${var+x} ]; then echo "var is unset"; else echo "var is set to '$var'"; fi
```

### Default Variables

Check if a variable is unset or null, then expand to default string value.

```bash
$ foo="bar"
$ echo "${foo:-defaultValue}"
↪ bar

$ echo "${fizz:-defaultValue}"
↪ defaultValue
```

Set a variable to default if a given variable is unset or null.

```bash { title="Set a default variable if the first argument is not supplied" }
# set default variable if first argument not supplied
VARIABLE_NAME="${1:-DEFAULT VALUE}"

# example usage
MESSAGE="${1:-Hello localhost}"
```

### Reading User Input

Read is used to get input from a user which can be set to a variable.

```bash { title="Get user input and set to variable" }
# set a variable
read VARIABLE_NAME
# set a variable, with prompt statement
read -p 'Enter name: ' VARIABLE_NAME
# set a variable, with prompt statement, password
read -sp 'Enter password: ' VARIABLE_NAME
```

### Exporting Varibles

Use `export` to make a variable available to a subprocess.

```bash { title="Use export to set a variable" }
export VAR_NAME=VALUE
```

---

## If Statements

```bash
if [[ $1 -gt 100]]
then
  echo "Some text"
fi
```

Use semicolons (`;`) to condense if statements down to one line.

```bash
# check if exists, otherwise set
if [ -z ${var+x} ]; then echo "var is unset"; else echo "var is set to '$var'"; fi
```

Extend logic with `if`, `elif`, and `else`.

{{< callout context="note" icon="👉" >}}
Extend logic with `if`, `elif`, and `else`.
{{< /callout >}}

```bash
if [ $1 -gt 100]
then
  echo "Some text"
elif [ $2 == 'no']
then
  echo "Alternate matching condition"
else
  echo "Final catch condition"
fi
```

### Wildcards

Use the asterisk wildcard (`*`) for partial matches of one or more occurences of any charachter. Can match beginning, end, or both.

```bash
if [[ $1 == *"$SUBSTRING"* ]]
then
  echo "Matches substring"
fi
```

Use the question mark (`?`) to match a single occurence of any chatachter.

```bash
if [[ $1 == ??"log.txt" ]]
then
  echo "Matches substring"
fi
```

### Boolean Operators

```bash
&&  # and
||  # or
```

```bash
if [[ $1 -gt 100 && $2 == 'yes']]
then
  echo "And condition met"
elif [[ $1 -gt 50 || $2 == 'no']]
then
  echo "Or condition met"
fi
```

### Equality Operators

```bash
=
==  # string comparisons
-eq # equals, numeric comparisons
-gt # greater than
-ge # greater than or equal to
-lt # less than
-le # less than or equal to
-ne # not equal
```

### Bracket Syntax

Bash has two types of syntax when dealing with if statements: `[single]` and `[[double]]`. Single is an older supported syntax and double allow for more features.

#### Single Bracket Notation

1. File based conditions `if [-L symlink]; then`
1. String-based conditions `if ["$some_var" == "foo"]; then`
1. Numeric-based conditions `if ["$some_var" -gt 100]; then`

#### Double Bracket Notation

1. **Equality operators**: introduces and (`&&`), or (`||`)
   - `if [[ "$some_var" == "test" && $1 -gt 2 ]]; then`
1. **Regex pattern matching**: introduces and (`&&`), or (`||`)
   - `if [[ "$some_var" == "test" && $1 -gt 2 ]]; then`
1. **Shell globbing**: asterisk will expand to anything
   - `if [[ "$some_var" == *[Ff]oo ]]; then`
1. **Word Splitting is prevented:** so quotes can be omitted around variables
   - `if [[ $some_var == *[Ff]oo ]]; then`
1. **Filenames are not expanded:**
   - With the older single bracked notation `if [ -a *.sh ]; then`. This will return true if there's a single matching shell file. Return false if there are no files. Return an error if
   - With double bracket notation, it will return true if there are one or more shell files
   - `if [[ $some_var == *[Ff]oo ]]; then`

---

## Redirection

```bash
# redirect stdout to file
command >file
# same as above
command 1>file
# redirect stdout to file (append to end of file)
command >>file
# redirect stderr to file
command 2>file
# redirect stdout & stderr to file
command &>file
# same as above
# redirect stdout out to file, redirect stderr to stdout, which outputs to file
command >file 2>&1
# discard stdout of command
command > /dev/null
# discard stdout & stderr of command
command &> /dev/null
# redirect contents of file to stdin of command
command <file
```

### Redirect Multiple Lines

```bash
command &lt;&lt;EOL
multi
line
text
goes
here
EOL
```

---

## Functions

### Defining and Calling Functions

```bash { title="Define and call a function" }
# define a function that takes an argument
function_name() {
  echo "Hello $1"
}

# call a function and pass an argument
function_name first_argument
```

### Returning Values

```bash { title="Set variable equal to return value of function" }
function_name() {
  local some_var='some value'
  echo "$(some_var)"
}

result=$(function_name)
```

```bash { title="Alternate: set variable equal to return value of function" }
function_name() {
  return 42
}

function_name
new_var=$?
```

### Handling Errors

```bash { title="Handling errors in function results" }
function_name() {
  return 1
}

if function_name; then
  echo "Great success"
else
  echo "Failure"
fi
```

---

## Loops

Loop there it is.

```bash
for i in TEST; do
  echo "$i"
done
```

### Ranges

While true...

```bash
for i in {1..5}; do
  ...
done
```

With step size

```bash
for i in {1..100..2}; do
  echo "Hello $i"
done
```

### Forever

While true...

```bash
while true; do
  ...
done
```

### Reading Lines

```bash
while read -r line; do
  echo $line
done <file_name
```

### Iterate Loop

```bash
for ((i = 0 ; i < 100 ; i++)) do
  echo $i
done
```

---

## Misc

```bash
echo "You're in $(pwd)"
```

Echo output and write to file

```bash
echo_and_write() {
  echo $* | tee -a file_name
}

echo_and_write "Some string to echo/write"
```
