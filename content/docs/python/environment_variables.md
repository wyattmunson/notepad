---
title: "Environment Variables and Flags"
description: ""
summary: "Checking and getting environment variables, command arguments, and flags in Python scripts."
date: 2024-10-22T21:17:23-07:00
lastmod: 2024-10-22T21:17:23-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Get Arguments

```python
import sys

for item in sys.argv:
  print(item)
```

```bash
$ python example.py

# OUTPUT
example.py
```

```bash
$ python example.py arg1 --flag='test' -p

# OUTPUT
example.py
arg1
--flag=test
-v
```
