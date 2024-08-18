---
title: "System"
description: ""
summary: ""
date: 2024-08-17T23:27:46-07:00
lastmod: 2024-08-17T23:27:46-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Bash system and hardware commands.

## Hardware

### List Block Devices

List all block devices, e.g., hard drives, attached to the system.

```bash
lsblk
```

```bash
$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda                         8:0    0 223.6G  0 disk
├─sda1                      8:1    0     1G  0 part /boot/efi
├─sda2                      8:2    0     2G  0 part /boot
└─sda3                      8:3    0 220.5G  0 part
  └─ubuntu--vg-ubuntu--lv 252:0    0 220.5G  0 lvm  /
sdb                         8:16   0  10.9T  0 disk
sdc                         8:32   0  10.9T  0 disk
sdd                         8:48   0  10.9T  0 disk
```
