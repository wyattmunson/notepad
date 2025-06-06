---
title: "File Structure"
description: ""
summary: "Explanation of root directory structure of Linux distributions. Kown as the Filesystem Heirarchy Standard (FHS)."
lead: ""
date: 2023-01-04T03:01:06-08:00
lastmod: 2023-01-04T03:01:06-08:00
draft: false
images: []
menu:
  docs:
    parent: "linux"
    identifier: "file-structure-a6e32610a8a0a8825e3f0256135f46b7"
weight: 999
toc: true
---

## Stanard Linux File Structure

This is known as FHS (Filesystem Hierarchy Standard).

### `/bin` - Binaries

Essential binaries for the system to run. Applications like Chrome would be stored in `/usr/bin`, while something like Bash would be stored in `/bin`.

### `/boot` - Boot files

Static boot files. The GRUB boot loader's files and Linux kernel are stored here. Boot loader config file are stored in `/etc` with the other configuration files.

### `/cdrom` - Old mount point for CD drives

While not part of the FHS standard, it still appears on Ubuntu and other distros. The standard location for media, however, is the `/media` directory.

### `/dev` - Device files

Linux exposes devices like hard drives as files. They're not traditional files in the technical sense, but they appear to the user as files. E.g, `/dev/sda` is the first SATA drive in the system.

### `/etc` - Configuration files

System wide condiguration files. User specific configurations are in the user's home directory.

### `/home` - Home folder

The `/home` directory contains the home folder for each user. Greg's home folder is found at `/home/benish`. It contains all the data files and config files for the user. Each user has wirte access to only their home folder and must elevate access to touch other files on the system.

### `/lib` - Essential shared libraries

Contains libraries needed by resources in `/bin` and `/sbin`.

### `/lost+found` - Recovered files

Corrupted files recovered if the system crashes.

### `/media` - Removeable media

Contains subdirectories where removeable media devices are mounted.

### `/mnt` - Temporary mount points

Where file systems where typically mounted. They can be mounted anywhere.

### `/opt` - Optional packages

Contains subdirectories for optional software packages. Used by proprietary software that doesn't use the standard file system heiarchy. Programs may dump their files in `opt/applicationName` when installed.

### `/proc` - Kernel & process files

Similar to the `/dev` directory, it does not contain standard files. They represent system and process information.

### `/root` - Root home directory

The home directory of the root user. Not located at `/home/root`. Nor is it `/`, which is the root directory.

### `/sbin` - System administration binaries

Similar to `/bin`, it contains binaries for system administration meant to be run by the root user.

### `/selinux` - SELinux virtual file system

If the distro uses SELinux (e.g., RedHat or Fedora) it contains related files.

### `/srv` - Service data

Contains data for services provided by the system. If you were running an Apache web server, you could store the files in the `/srv` directory.

### `/tmp` - Temporary files

Contains temporary files stored by applications. Generally removed on restart, my be deleted at anytime by utilities like `tmpwatch`.

### `/usr` - User binaries and read only data

Contains applicaions and files used by the user (instead of by the system). Non-essential applications are located in `/usr/bin`. Libraries are located in `/use/lib`. There are a few others.

### `/var` - Variable data files

Contains
