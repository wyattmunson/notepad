---
title: "cd"
description: "Probably the most rudimentary command. Allows you to traverse the file system."
summary: ""
date: 2025-01-16T02:28:48-08:00
lastmod: 2025-01-16T02:28:48-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Probably the most rudimentary command. Allows you to traverse the file system.

```bash
$ pwd                       # print working directory - get current location
↪ /users/benish/code

$ ls                        # list out content of current directory
↪ file.txt  lounge-frontend/  lounge-backend/

$ cd lounge-frontend/       # move into "lounge-frontend"
# pwd /users/benish/code/lounge-frontend

$ cd ..                     # move up one directory
# pwd /users/benish/code

$ cd -                      # return to previous directory
# pwd /users/benish/code/lounge-frontend
```
