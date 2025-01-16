---
title: "touch"
description: "Touch command creates a new file."
summary: ""
date: 2025-01-16T02:26:46-08:00
lastmod: 2025-01-16T02:26:46-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Touch command creates a new file.

```bash
touch FILE_NAME
```

```bash
# create multiple files
touch FILE_NAME_1 FILE_NAME_2
```

**Full argument list**

- `-A` Adjust access and modification timestamps of value with specified value.
- `-a` Change access time of file. Modification time is not changed, unless `-m` flag also specified.
- `-c` Do not create the file if it does not exist
- `-d` Change access and modification times
- `-h` If file has symbolic link, change time of the link itself, rather than the file the link points to. Using `-h` implies `-c` and will not create a new file.
- `m` Change modification time of file. Access time is not changed unless `-a` is also specified.
- `r` Use access and modification times from specified file instead of the current time of day.
- `t` Change access and modification times to the specified time instead of the current time of day.

```bash
# Adjust access and modification timestamps of value with specified value
touch

# Change access time of file. Modification time is not changed, unless `-m` flag also specified.
touch -a FILE_NAME

```
