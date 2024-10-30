---
title: "Environment Variables and Flags"
description: ""
summary: "Checking and getting environment variables, command arguments, and flags in Python scripts."
date: 2024-10-22T21:17:23-07:00
lastmod: 2024-10-22T21:17:23-07:00
weight: 31
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Get Arguments

{{< callout context="note" title="Get all arguments" icon="outline/code" >}}
Use `sys.argv` to get all arguments supplied to the Python script.
{{< /callout >}}

Use `sys.argv` to get all the arguments that were provided to the python script when it was invoked. It returns an array with all the arguments supplied, in order.

```python
import sys

for item in sys.argv:
  print(item)
```

The first item in the array from `sys.argv` is the name of python script that was called.

```bash
$ python example.py

# OUTPUT
example.py
```

The `sys.argv` command will return an array that contains all the commands supplied to the python command in the order they were provided.

```bash
$ python example.py arg1 --flag='test' -p

# OUTPUT
example.py
arg1
--flag=test
-v
```

### Check if env variable exists, otherwise get input

Check if an environment variable exists, otherwise get user input. Additionaly, the below code checks for a `KeyError`.

```python
source_account = os.environ.get('SOME_VAR') if not KeyError else input("Enter input:")
```

### Check for flag and return value

The following code checks `sys.argv` to see if a flag was provided and return the value if so.

The expected flag sytax is `--flag-name="some value"`. If `--flag-name` exists it will return the value of `some value`.

```python
def find_flag(flag_name):
    flag_value = None
    for item in sys.argv:
        if item.startswith(flag_name):
            flag_value = item.replace(flag_name + "=", "")
            print(f"Found flag '{flag_name}' with value of '{flag_value}'")
            return flag_value
    return flag_value

find_flag("--stylesheets")
```

### Check to see if flag exists

The following code checks `sys.argv` to see if a flag was provided and return true if so.

The expected flag sytax is `--flag-name`. If `--flag-name` exists it will return `true`. A value is not expected, but will be safely ignored if provided.

```python
def check_flag_exists(flag_name):
    for item in sys.argv:
        if item.startswith(flag_name):
            return True

    return False

check_flag_exists("--silent")
check_flag_exists("-s")
```
