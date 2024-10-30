---
title: "Python Snippets"
description: ""
summary: "General Python code snippet repository."
lead: ""
date: 2024-06-03T22:35:44-07:00
lastmod: 2024-06-03T22:35:44-07:00
draft: false
weight: 210
images: []
menu:
  docs:
    parent: ""
    identifier: "python-snippets-bc2c41697207056757ca8a0ac270807b"
toc: true
---

Snippets of various python code.

## Time

Things of time.

### Convert datetime to string

#### Convert datetime to string - `strftime()`

```python
from datetime import datetime

now = datetime.now()
now.strftime("%Y")
```

#### Convert string to datetime (`strptime()`)

```python
from datetime import datetime

date_str = ""
datetime.strptime(date_str, "%d")
```

## Random

### Handle subprocess.run

```python
import subprocess
import logging

def call_subprocess(cmd, capture_output=True):
  """
  Runs a subprocess command and captures errors with logging.

  Args:
      cmd: A list containing the command and its arguments.
      capture_output: Whether to capture standard output and error (default: True).

  Returns:
      A CompletedProcess object on success, None on failure.
  """
  try:
    # Call subprocess.run with desired arguments
    result = subprocess.run(cmd, capture_output=capture_output)
    return result
  except subprocess.CalledProcessError as error:
    # Log error with specific message for CalledProcessError
    logging.error(f"Subprocess failed with exit code: {error.returncode}")
    logging.error(f"Command: {' '.join(cmd)}")
    if capture_output:
      logging.error(f"Standard Error: {error.stderr.decode('utf-8')}")
  except (FileNotFoundError, PermissionError) as error:
    # Log more specific errors for common exceptions
    logging.error(f"Error: {type(error).__name__} - {error}")
    logging.error(f"Command: {' '.join(cmd)}")
  # No return here if there's an exception

# Example usage
command = ["ls", "/nonexistent/directory"]
try:
  result = call_subprocess(command)
  if result:
    print(f"Subprocess output: {result.stdout.decode('utf-8')}")
except Exception as e:
  # Catch any other unexpected exceptions
  logging.error(f"Unexpected error: {e}")
```

## Install Deps with Pip

```python
# install deps
pip install -r requirements.txt

# updated deps
pip install --upgrade -r requirements.txt
```

## Start Virtual Environment with `venv`

```python
# create virtual env
python -m venv <environment-name>
python -m venv venv

# activate environment
source <environment-name>/bin/activate
source venv/bin/activate

# install deps
pip install -r requirements.txt

# deactive environment
deactivate

# remove environment
rm -rf venv
```

# Terminal Subprocess

Keys: Terminal, Bash, run shell command, run command

```python
import subprocess

```

### Get user input, translate input value

```python
class UI:
  def display_menu(self, menu_name):
    # ...

  def get_user_input(self):
    # Get user input
    user_choice = input("Enter your choice: ")

    # Simple translation based on a dictionary
    menu_options = {
        "1": "payments",
        "2": "settings",
    }
    return menu_options.get(user_choice, user_choice)  # Handle invalid choices
```

### Find in array

```python
my_array = [1, 2, 3, 4, 5]
value_to_find = 3

if value_to_find in my_array:
  print("Value exists in the array")
else:
  print("Value not found")
```

### Check if multiple variables are all populated

```python
variable1 = 10
variable2 = "hello"
variable3 = None  # Set one variable to None

if all(variable1, variable2, variable3):
  print("All variables are populated")
else:
  print("At least one variable is None")
```

### String Interpolation

```python
some_string = "Hello there"
print(f"The string is: {some_string}"
```

### For Loop and For Loop with index/iteration

```python
for x in options:
          print(x)

for index, x in enumerate(options):
	print(index, x)
```

### Starts with

Find item that starts with the same text, fuzzy matching:

```bash
my_list = ["--source", "--target", "--config=default"]
my_string = "--source=123"

# Use enumerate to get both the item and its index
for index, item in enumerate(my_list):
  # Check if the string starts with the list item
  if my_string.startswith(item):
    print(f"Partial match for '{my_string}' found at index {index}")
    break  # Exit the loop after finding the first match

# If the loop completes without a break, no match is found
else:
  print(f"String '{my_string}' not found in the list")
```

### Dynamically update class variables

```python
def update_config(self, config_name, value):
       print(config_name)
       if hasattr(self, config_name):
          setattr(self, config_name, value)
       else:
        print(f"Error: Attribute '{config_name}' does not exist.")
```

## Rand Snip

```python
msg = f"Enter source account id [{existing_configs['source_account']}]: "

```

## Argument Names

```python
def getPerson(name, age, state):
	print(name, age, state)

getPerson("Greg", 90, "VA")
getPerson("Greg", state="VA", age=90)
```

## Check to see if attribute exists:

```python
def configureWireless(self):
        # check setPipes was set
        if not hasattr(self, "transmit"):
            print("Error message for you to see.")
            print("ERROR: Call setPipes() to define transmit address")
            raise ValueError("Transmit not set. See more information above.")
```

## Kargs with methods

METHOD”

```python
def configureWireless(self, **kwargs):

  transmit = kwargs.get("transmit")
  receive = kwargs.get("receive")

  if not transmit or not receive:
    print("ERROR: Transmit or recieve not set")
    raise ValueError("Transmit or recieve not set")
```

Calling Method

```python
my_object = MyObject()

my_object.configureWireless(transmit="192.168.1.1", receive="192.168.1.2")
```

## Converting Hex

```python
# convert hex to bytes
print("TO BYTES", bytes.fromhex("d2f0f0f0f0"))
>>> TO BYTES b'\xd2\xf0\xf0\xf0\xf0'

# Convert hex to decimal
print("TO DECIMAL", int("d2f0f0f0f0", 16))
>>> TO DECIMAL 905985454320

print("TO HEX", bytes.hex(tx))
>>> TO HEX d2f0f0f0f0
```

## Arrays

```python

```

## JQ

### Convert Each Line to Array

```python
sudo systemctl status docker | head | jq --raw-input --slurp 'split("\n")'
```

### Split Columns into JSON

```python
sudo docker ps | jq -nR '[inputs | split(" ") | { "containter_id": .[0] } ]'
```

## Randomly select element in array

```python
import random

random.choice([1, 2, 3])

# Get random int between two given ints
random_int = random.randint(0, len(tickets))
random_int = random.randint(0, 9)
```

## Loop though a range - repeat a given number of times

```python
for x in range(5):
	print(x)
```

## Regex Match

### See if string matchs "Xs" format

Use regex to see if a string matches the format of "nd", where "n" is a number, and "d" is the letter "d". It will match strings like `2d` or `16d`.

```python
def is_match(string):
    return bool(re.fullmatch(r"\d+d", string))
```

#### Match multiple charachters

The following code updates the regex to match a string ending in "d" or "w".

```python
def is_match(string):
    return bool(re.fullmatch(r"\d+[dw]", string))
```
