---
title: "Kill"
description: "Stop a process by process ID."
summary: ""
date: 2025-01-16T02:19:21-08:00
lastmod: 2025-01-16T02:19:21-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Stop a process by process ID.

```bash
# see processes running in terminal
$ ps
  PID TTY           TIME CMD
27894 ttys001    0:00.08 -bash

# see process for all users and outside of terminal
$ ps aux
$ ps aux | grep chrome


$ kill SIGNAL PID
$ kill -9 ./applications-service

# see all signal commands
# 9 (SIGKILL) - is the most common command to terminate a process
$ kill -l
```
