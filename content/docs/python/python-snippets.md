---
title: "Python Snippets"
description: ""
summary: "General Python code snippet repository."
lead: ""
date: 2024-06-03T22:35:44-07:00
lastmod: 2024-06-03T22:35:44-07:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "python-snippets-bc2c41697207056757ca8a0ac270807b"
weight: 999
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

### Open File Modes

- `r` - read only. I/O exception raised if file doesn’t exist. Default mode. Handle location is start of file.
- `r+` - read and write. I/O exception raised if file doesn’t exist. Default mode. Handle location is start of file.
- `w` - write only. Existing files overwritten. Creates new file if doesn’t exist. Fails if writing to new directory that doesn’t exist. Handle location is start of file.
- `w+` - ready and write. Overwrites existing text.
- `a` - append only. Writes new text at end of file. Creates new file if doesn’t exist. Fails if writing to new directory that doesn’t exist. Handle location is end of file.
- `a+` - read and write. Read and append behavior.

```python
try:
	with open(filename, "a") as file_obj:
		file_obj.write(string_to_append)
		print(f"Appended {string_to_append}")
except FileNotFoundError:
	print("Error")
```

### Create a directory

- When using `os.makedirs()` if not using the aboslute path, e.g., calling the python script from a different path than the code is in, it will fail.

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

# Date / Time

```python
from datetime import datetime
now = datetime.now()

year = now.strftime("%Y") # => 2018

week = now.strftime("%a") # => Sun
week = now.strftime("%A") # => Sunday
week = now.strftime("%w") # => 0

day = now.strftime("%d") # => 01
day = now.strftime("%-d") # => 1

month = now.strftime("%b") # => Jan
month = now.strftime("%B") # => January
month = now.strftime("%m") # => 01
month = now.strftime("%-m") # => 1

hour = now.strftime("%H") # => 00..23
hour = now.strftime("%-H") # => 0..23
hour = now.strftime("%I") # => 01...12
hour = now.strftime("%=I") # => 1..12
meridian = now.strftime("%H") # => AM..PM
minute = now.strftime("%M") # => 00..59
minute = now.strftime("%-M") # => 0..59
second = now.strftime("%S") # => 00..59
second = now.strftime("%-S") # => 0..59
microsecond = now.strftime("%f") # => 00000..99999

utc_offset = now.strftime("%z") # => 00..59
time_zone_name = now.strftime("%Z") # => 0..59ode

local_timestamp = now.strftime("%c") # => Mon Jun  3 17:40:16 2024
```

### Convert Times

```python
from datetime import datetime

# convert datetime to string
strftime("%Y")

# convert string to datetime
datetime.strptime(date_string, "%d")
```

# Terminal Subprocess

Keys: Terminal, Bash, run shell command, run command

```python
import subprocess

```

# Classes

Good Guide: https://realpython.com/python-classes/#getting-started-with-python-classes

### Class with state manager

```python
class State:
  def __init__(self):
    self.current_menu = "main"
    self.user_data = {}  # Store any relevant user data

  def update_state(self, selection):
    # Update state based on user selection
    # ...

  def get_next_action(self):
    # Determine next action based on current state
    # ...

class UI:
  def display_menu(self, menu_name):
    # Display menu options from state
    # ...

  def get_user_input(self):
    # Get user input and validate it
    # ...

class Logic:
  def perform_action1(self, data):
    # Business logic for action 1
    # ...

  def perform_action2(self, data):
    # Business logic for action 2
    # ...

class App:
  def __init__(self):
    self.state = State()
    self.ui = UI()
    self.logic = Logic()

  def run(self):
    while True:
      self.ui.display_menu(self.state.current_menu)
      user_input = self.ui.get_user_input()
      self.state.update_state(user_input)
      next_action = self.state.get_next_action()
      if next_action == "action1":
        self.logic.perform_action1(self.state.user_data)
      elif next_action == "action2":
        self.logic.perform_action2(self.state.user_data)
      # ... handle other actions ...
      elif next_action == "exit":
        break

if __name__ == "__main__":
  app = App()
  app.run()

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

### Check if env variable exists, otherwise get input

```python
source_account = os.environ.get('SOME_VAR') if not KeyError else input("Enter input:")
```

Know, working

```python
def check_var(var, prompt):
           res = os.environ.get(var)
           if res is None:
              print("ERROR: Environment variable {var} not supplied")
              res = input(prompt)
           print(f"INFO: Got {var} from environment variable")
           return res

check_var("SOME_VAR", "Enter some variable:")
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

# Error Handling

- **ValueError:** Raised when a correct argument type but an incorrect value is supplied to a function. For example, if you try to pass a string to a function that expects an integer, a ValueError will be raised.
- **TypeError:** Raised when an argument is of the wrong type. For example, if you try to pass a string to a function that expects an integer, a TypeError will be raised.
- **IndexError:** Raised when an index is out of range. For example, if you try to access the 10th element of a list that only has 5 elements, an IndexError will be raised.
- **KeyError:** Raised when a key is not found in a dictionary. For example, if you try to access the key "foo" in a dictionary that does not have a key "foo", a KeyError will be raised.
- **NameError:** Raised when a name is not defined. For example, if you try to use the variable "foo" but the variable "foo" has not been defined, a NameError will be raised.

## Check to see if attribute exists:

```python
def configureWireless(self):
        # check setPipes was set
        if not hasattr(self, "transmit"):
            print("Error message for you to see.")
            print("ERROR: Call setPipes() to define transmit address")
            raise ValueError("Transmit not set. See more information above.")
```

## Set Variables in try catch

A variable can be set in a try/except assuming the variable is also defined in the except or an error is raised.

```python
try:
	spi = SPI(0, sck=Pin(2), mosi=Pin(7), miso=Pin(4))
	cfg = {"spi": spi, "csn": 3, "ce": 0}
except ValueError as e:
	print("Upstream error: ", e)
	raise ValueError("Last error rendered to user")

# spi can be defined in "try"
# spi can be defined in "except" or raise Error
print(spi)
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
