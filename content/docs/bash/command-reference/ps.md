---
title: "ps"
description: "Use ps to see running processes."
summary: ""
date: 2025-01-16T02:22:37-08:00
lastmod: 2025-01-16T02:22:37-08:00
draft: true
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Use to see running processes.

```bash
# show running terminal sessions
ps
```

### Show all running processes

```bash
ps aux

# use grep to find process by keyword
ps aux | grep keyword
```

```bash
# check if a process is running
ps auxwww | grep postgres
```
