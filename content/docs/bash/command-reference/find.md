---
title: "find"
description: "Use find to get matching files and directories."
summary: ""
date: 2025-01-16T01:08:31-08:00
lastmod: 2025-01-16T01:08:31-08:00
weight: 107
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Find a file or directory by name. Will search current directory and all subdirectories at a given file path.

Basic usage:

```bash
# search for matching files or directory names (and subdirectories)
find . -name search_text
```

Usage:

```bash
# ignore text case (case-insensitive search)
find . -iname search_text

# wildcard search
find . -name "*.jpg"
find . -name "tmp.???"

# delete matching files/directories
find . -name "tmp*" -delete

# follow subdirectories to level X
find . -name search_text -maxdepth X
# another flag
find . -name search_text -depth X

# only find files modified in last 4 days
find . -name search_text -mtime 4

# SEARCH BY TYPE
# only return type file
find . -name search_text -type f
# only return type directory
find . -name search_text -type d

# SEARCH BY SIZE
# files over 1GB
find . -name search_text -size +1G
# files below 100MB
find . -name search_text -size -100M
```
