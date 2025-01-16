---
title: "ls"
description: ""
summary: ""
date: 2025-01-16T02:19:49-08:00
lastmod: 2025-01-16T02:19:49-08:00
weight: 240
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

List contents of a directory.

List current directory:

```bash
ls
```

List a specified directory:

```bash
ls DIRECTORY
ls /local/usr/bin
```

`ls` flags:

```bash
ls -l
ls -lah

# flags
  # list/table view with date, ownership, and size
  -l
  # list hidden files like .git (starting with a dot ".")
  -a
  # human readable file sizes
  -h
  # list subdirectories
  -R
  # order by last modified
  -t
  # order by file size
  -lS
  # list in reverse order (combine with other commands)
  -r
  # show directories with "/" and executables with "*"
  -F
  # display inode number of file or directory
  -i
```
