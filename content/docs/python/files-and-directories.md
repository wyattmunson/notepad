---
title: "Files and Directories in Python"
description: ""
summary: "Using Python to manipulate files and directories. Create, check for, move, and write to files."
date: 2024-10-29T21:56:08-07:00
lastmod: 2024-10-29T21:56:08-07:00
weight: 27
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Create Directory

{{< callout context="note" title="Create directory" icon="outline/terminal-2" >}}
Use `os.mkdir()` to create a directory.
{{< /callout >}}

```python { title="Model" }
# syntax
os.mkdir(DIR_PATH)

# usage
os.mkdir("/tmp/some/path")
```

{{< callout context="caution" title="Create directory" icon="outline/alert-triangle" >}}
It's a good practice to specify a absolute file path.
{{< /callout >}}

### Basic Usage

```python
import os

dir_name = "/some/dir"

try:
    os.mkdir(dir_name)
except FileExistsError:
    print(f"Directory '{dir_name}' already exists.")
except PermissionError:
    print(f"Permission denied: Unable to create '{dir_name}'.")
except Exception as e:
    print(f"An error occurred: {e}")
```

```bash
# OUTPUT
['sys', 'run', 'tmp', 'boot', 'mnt', 'dev', 'proc', 'var', 'bin', 'lib64', 'usr',
'lib', 'srv', 'home', 'etc', 'opt', 'sbin', 'media']
```

## List Files and Directories

{{< callout context="note" title="List files" icon="outline/terminal-2" >}}
Use `os.listdir()` to list all directories and files.
{{< /callout >}}

Use `os.listdir()` to list all directories and files at the given file path.

```python { title="Model" }
# syntax
os.listdir(DIR_PATH)

# usage
os.listdir("/tmp")
```

### Basic Usage

```python
import os

os.listdir(source_dir)
```

The output is an array of the current directory path.

## Copy File

{{< callout context="note" icon="outline/terminal-2" >}}
Use `shutil.copyfile()` to copy a file.
{{< /callout >}}

Use `shutil.copyfile()` to copy a file. The source and target must both be files.

```python { title="Model" }
# syntax
shutil.copyfile(SOURCE_FILE, TARGET_FILE)

# usage
shutil.copyfile("/tmp/file.txt", "/home/file.txt")
```

### Basic Usage

```python
import shutil

try:
    shutil.copyfile(original_file_loc, new_file_loc)
except:
    print("==> Failed to transfer file:", original_file_loc)
```

## Open, Read, and Write to Files

```python { title="Model" }
# syntax
open(FILE_NAME, FILE_MODE) as file_obj:

# usage
open("/tmp/file.txt", "a") as file_obj:
```

## Write to a File

Use Python to write to and read from a file. Use the file mode to specify if Python should read, write, or append to the file.

```python
filename = "notes.txt"

try:
	with open(filename, "a") as file_obj:
		file_obj.write(string_to_append)
		print(f"Appended {string_to_append}")
except FileNotFoundError:
	print("Error")
```

### Open File Modes

The file mode dictates if the script can read, write, append, or some combination thereof. The following list contains the different possible file modes:
| File Mode | Permission | Description |
| --- | --- | --- |
| `r` | Read only. | I/O exception raised if file doesn’t exist. Default mode. Handle location is start of file. |
| `r+`| Read and write. | I/O exception raised if file doesn’t exist. Default mode. Handle location is start of file. |
| `w` | Write only. | Existing files overwritten. Creates new file if doesn’t exist. Fails if writing to new directory that doesn’t exist. Handle location is start of file. |
| `w+`| Ready and write. | Overwrites existing text. |
| `a` | Append only. | Writes new text at end of file. Creates new file if doesn’t exist. Fails if writing to new directory that doesn’t exist. Handle location is end of file. |
| `a+`| Read and write. | Read and append behavior. |

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
