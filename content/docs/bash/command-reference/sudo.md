---
title: "sudo"
description: "sudo allows a user to execute a command as the superuser or another user, within the specified security policy."
summary: ""
date: 2025-01-16T02:20:53-08:00
lastmod: 2025-01-16T02:20:53-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## `sudo` - Super User DO

`sudo` allows a user to execute a command as the superuser or another user, within the specified security policy.

If a command fails because of insufficient prviledges, the `sudo` command can help. This does elevate permissions and should be used with caution.

```bash
sudo COMMAND
sudo rm -rf /data
```

More:

```bash
# add a new user
sudo useradd

# create password for new user
sudo passwd

# add to a group
sudo groupadd

# delete user
sudo userdel

# delete group
sudo groupdel

# add user to a primary group
sudo usermod -g
```
