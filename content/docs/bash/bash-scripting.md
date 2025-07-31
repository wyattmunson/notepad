---
title: "Bash Scripting"
description: ""
lead: ""
summary: "Bash scripting information on variables, conditionals, redirection, looping, and functions."
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

# unset a variable
unset $VARIABLE_NAME
```

### Save Variable to File

```bash
# set variable `TOKEN` to file `.token` (overwrites file)
echo "$TOKEN" > .token
# adds variable `TOKEN` to file `.token` (appends to file)
echo "$TOKEN" >> .token
# optionally unset token after saving it
unset TOKEN
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
# return true if variable exists AND is set to a value
if [ -n "$1" ]; then
  echo "You supplied the first parameter!"
else
  echo "First parameter not supplied. Exiting."
  exit 1
fi

# returns true if variable does not exist OR not set to a value
if [ -z "$1" ]; then
  echo "Does not exists"
fi

# check if exists, otherwise set
if [ -z ${var+x} ]; then echo "var is unset"; else echo "var is set to '$var'"; fi

# only supported in bash 4.2 (not widely supported)
if [ -v "$1" ]; then
  echo "Does not exists"
fi
```

- The `-n` operator returns true if string's length is non-zero, meaning it's set to some value.
- The `-z` operator returns true if length of string is zero. Meaning it's set to nothing or doesn't exist at all.

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

### Exporting Variables

Use `export` to make a variable available to a subprocess.

```bash { title="Use export to set a variable" }
export VAR_NAME=VALUE
```

### Exporting Variables using `eval`

When a child script is called using `eval`, use the below code in the child script to make the variables in the parent script.

```bash { title="Use echo export to set a variable" }
# export EVAL_EXPORT for use in parent script
echo 'export TEST_EVAL_EXPORT="filled"'

# set NEXT_VERSION equal to the variable VERSION in the child script
echo "export NEXT_VERSION=\"$VERSION\""
```

```bash { title="Calling child script from parent script using eval" }
# Calling child script from parent script using `eval`
eval "$("/opt/winc/semver/scripts/main.sh")"

# Use tee to capture the stdoout of the the child script
# Use grep so only the echo statements starting with "export" are captured as an env var
eval "$("/opt/winc/semver/scripts/main.sh" | tee /dev/stderr | grep '^export ')"
```

---

## If Statements

```bash
if [[ $1 -gt 100 ]]
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
if [ $1 -gt 100 ]; then
  echo "Some text"
elif [ $2 == 'no']; then
  echo "Alternate matching condition"
else
  echo "Final catch condition"
fi
```

### Equality Operators

{{< callout context="tip" title="Equality Operators" >}}

{{< /callout >}}

| Operator | Usage                             |
| -------- | --------------------------------- |
| =        | equal to                          |
| ==       | string comparisons (non-standard) |
| !=       | not equal to                      |
| -eq      | equals, numeric comparisons       |
| -gt      | greater than                      |
| -ge      | greater than or equal to          |
| -lt      | less than                         |
| -le      | less than or equal to             |
| -ne      | not equal, numeric comparisons    |

#### String Comparisons

```bash
=   # equal to
==  # string comparisons (non-standard)
!=  # not equal to
```

```bash { title="Check if two strings are equal" }
if [ "$foo" = "bar" ]; then echo "Foo is set to bar"; fi
```

- Checks if the variable `$foo` is equal to the string of `bar`.
- Note the single equal sign (`=`).

```bash
if [ "$foo" != "bar" ]; then echo "Foo is set to bar"; fi
```

#### Numeric Comparisons

Bash uses different operators to compare two numbers.

```bash
-eq # equals, numeric comparisons
-gt # greater than
-ge # greater than or equal to
-lt # less than
-le # less than or equal to
-ne # not equal
```

```bash
foo=50
if [ "$foo" -gt 10 ]; then echo "Foo is greater than 10"; fi
```

### Wildcards

{{< callout context="tip" title="Wildcards" icon="outline/alert-triangle" >}}
Use wildcards to perform partial string matches.

- `*` - one or many occurrences of any character
- `?` - single occurrence of any character
- `[]` - single occurrence of one character inside the brackets
  {{< /callout >}}

#### Asterisk `*` Wildcard

Use the asterisk wildcard (`*`) for partial matches of one or more occurrences of any character. Can match beginning, end, or both.

```bash { title="Asterisk wildcard basic usage" }
if [[ $1 == *"pattern"* ]]; then
  echo "Matches substring"
fi

# Use variables for search pattern
if [[ $1 == *"$SEARCH_PATTERN"* ]]; then
  echo "Matches substring"
fi

# quotes are not required
if [[ $1 == *log*.txt ]]; then echo "Matches"; fi
# ==> matches `log-01.txt`, `syslog-01.txt`
```

#### Question Mark `?` Wildcard

Use the question mark (`?`) to match a single occurrence of any character.

Use multiple question marks to

```bash
# If using quotes, `?` must appear outside
if [[ $1 == ?"log.txt" ]]; then echo "Matches"; fi
# ==> matches `alog.txt`

# Paramaterize the search pattern
if [[ $1 == ?"$FILE_NAME" ]]; then echo "Matches"; fi
# ==> matches variable $FILE_NAME

# use multiple quotes to match multiple characters
if [[ $1 == ???"log.txt" ]]; then echo "Matches"; fi
# ==> matches `syslog.txt`

# quotes are not required
if [[ $1 == log-??.txt ]]; then echo "Matches"; fi
# ==> matches `log-01.txt`
```

{{< callout context="caution" >}}
Wildcards like `*` and `?` must appear outside of quotation marks.
{{< /callout >}}

```bash
# NOT VALID: wildcard will not work
if [[ $1 == "????log.txt" ]]; then echo "Matches"; fi
```

Use the bracket wildcard (`[]`) to match a single occurrence of one of the characters match.

```bash
# Match single occurrence of a single character in the []
if [[ $1 == [Ee]xample ]]; then echo "Matches"; fi
# ==> matches `Example` or `example`
# ==> no match `EExample`
```

Note: double required for bracket expression

### More String Comparisons

#### Regex String Comparisons

```bash
if [[ "$str" =~ ^[a-zA-Z]+$ ]]; then echo "str is alphabetic"; fi
```

#### String Length Comparisons

Use the `#` operator to compare string length.

```bash
# check if variable `foo` is shorter than `bar`
if [[ "${#foo}" -lt "${#bar}" ]]; then echo "foo is shorter"; fi
```

### Boolean Operators

```bash
&&  # and
||  # or
```

```bash
if [[ $1 -gt 100 && $2 == 'yes']]; then
  echo "And condition met"
elif [[ $1 -gt 50 || $2 == 'no']]; then
  echo "Or condition met"
fi
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

## Set

- `set -u` - attempts to use undefined variables as errors
- `-e` - exits immediately if a command fails
- `-x` - enable debugging by printing every command before it is executed
- `-o pipefail` - exit status of first non-zero exit code in a command pipeline (`cmd1 | cmd2 | cmd3`). Normal behavior is to return the exit code of the last command.

```bash
DEBUG_MODE=true
if [ "$DEBUG_MODE" != "true" ]; then
    set -euxo pipefail
fi
```

```bash
DEBUG_MODE=true
if [ "$DEBUG_MODE" != "true" ]; then set -euo pipefail; else set -o pipefail; fi
```

---

## Data Types

### Arrays

```bash
VAR_NAME=("foo" "bar")

for x in ${VAR_NAME[@]};
do
    # your commands here
done
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

### Loop over directories

- Given directory is `/tmp`
- With subdirectories `/tmp/A`, `/tmp/B`, `/tmp/C`
- It will return `A`, `B`, `C`

```bash { title="Return names of subdirectories" }
for dir in /tmp/*/     # list directories in the form "/tmp/dirname/"
do
    dir=${dir%*/}      # remove the trailing "/"
    echo "${dir##*/}"    # print everything after the final "/"
done
```

```bash
# Loop through each subdirectory
for dir in "$1"/*/; do
  if [ -d "$dir" ]; then
    echo "Processing directory: $dir"

    # Change to the subdirectory
    cd "$dir" || continue

    # Loop through each album
    for alb in "$dir"/*/; do
        echo "Processing Album: $alb"
    done

    # Go back to the original directory
    cd - > /dev/null
  fi
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
