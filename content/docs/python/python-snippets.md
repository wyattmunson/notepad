---
title: "Python Snippets"
description: ""
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
