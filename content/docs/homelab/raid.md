---
title: "RAID"
description: ""
summary: ""
date: 2024-08-18T00:52:41-07:00
lastmod: 2024-08-18T00:52:41-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Roll your own RAID setup.

This guide will establish a RAID 5 array on an Ubuntu Server system.

## Setup

Before creating a RAID array, each drive must be partitioned and formatted.

### List block devices

```bash { title="View block devices" }
$ lsblk
NAME                  	MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda                     	8:0	0 223.6G  0 disk
├─sda1                  	8:1	0 	1G  0 part /boot/efi
├─sda2                  	8:2	0 	2G  0 part /boot
└─sda3                  	8:3	0 220.5G  0 part
  └─ubuntu--vg-ubuntu--lv 252:0	0 220.5G  0 lvm  /
sdb                     	8:16   0  10.9T  0 disk
sdc                     	8:32   0  10.9T  0 disk
sdd                     	8:48   0  10.9T  0 disk
```

In the above example:

- `sda` - the disk the OS is installed on
- `sdb`, `sdc`, `sdd` - the HDD that will be used in the RAID array

{{< callout note >}}
In general the devices are referenced as `/dev/sdb`.
{{< /callout >}}

### Create a New Partition

To begin, a partition must be created on the drive.

Use `fdisk` to create the partitions. It will launch an interactive session.

{{< callout context="tip" icon="outline/bulb" >}}
Remember to append `/dev` to the drive names when using `fdisk`. E.g., the drive `sdb` from the `lsblk` command will be referenced as `/dev/sdb` using the `fdisk` command.
{{< /callout >}}

```bash { title="Use fdisk to create a partition" }
sudo fdisk /dev/sdb
```

- `sudo fdisk /dev/sdb` - Select `sdb` drive
- `g` - use GPT partition table
- `n` - create new partition
- `1` - create first partition
- `ENTER` - use default for first sector
- `ENTER` - use default for last sector
- `w` - write (save)

### Validate Partitions are Created

Use the `lsblk` command to verify each disk has a partition associated with it.

{{< details "Output of lsblk" >}}

```bash { title="Use lsblk to view partitions" }
NAME                      MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda                         8:0    0 223.6G  0 disk
├─sda1                      8:1    0     1G  0 part  /boot/efi
├─sda2                      8:2    0     2G  0 part  /boot
└─sda3                      8:3    0 220.5G  0 part
  └─ubuntu--vg-ubuntu--lv 252:0    0 220.5G  0 lvm   /
sdb                         8:16   0  10.9T  0 disk
└─sdb1                      8:17   0  10.9T  0 part
sdc                         8:32   0  10.9T  0 disk
└─sdc1                      8:33   0  10.9T  0 part
sdd                         8:48   0  10.9T  0 disk
└─sdd1                      8:49   0  10.9T  0 part
```

{{< /details >}}

Each disk should now have a partition under it. For example, the `sdb` disk should have a `sdb1` partition under it. Verify the disk partition is the same size of the disk.

{{< picture src="images/homelab/lsblk_partition.png" alt="A bird flying through a field of tall grass" >}}

## Create RAID Array

### Install `mdadm` Utility

```bash { title="Install mdadm with apt" }
sudo apt install mdadm -y
```

### Create RAID 5 Array

```bash { title="Install mdadm with apt" }
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb1 /dev/sdc1 /dev/sdd1
```

{{< callout context="caution" icon="outline/alert-triangle" >}}
Creation of the RAID array may take several hours.
{{< /callout >}}

#### Monitor creation of RAID array

Monitor creation of RAID devices.

```bash { title="Check status of RAID array creation" }
cat /proc/mdstat
```

Use `watch` to refresh status of process every 2 seconds.

```bash { title="Watch status of RAID array creation" }
watch cat /proc/mdstat
```

### Inspect RAID 5 device

Inspect RAID 5 array after creation.

```bash { title="View RAID 5 array" }
mdadm --detail /dev/md0
```

## Create Filesystem

Using the /dev/md0 device, create a ext4 filesystem.

### Create Filesystem with ext4 format

Inspect status of each device (drive) with mdadm.

```bash { title="Use mkfs to create filesystem" }
mkfs.ext4 /dev/md0
```

Verify creation of filesystem with lsblk command.

### Mount newly created device

Mount the device using the mount command.

```bash { title="Use mount command to mount filesystem" }
mount /dev/md0 /mnt
```

Verify the device is mounted with df.

### Persist device across reboots

Update the `/etc/fstab` file to persist the mount point across reboots.

```bash { title="Check /etc/fstab" }
$ cat /etc/fstab
/dev/md0 /mnt ext4 defaults 0 0
```

## Persist RAID Configuration Across Reboots

By default, there is no RAID configuration. On reboot, the RAID configuration is not guaranteed to be in md0 rather something else.

### Save RAID configuration to file

```bash { title="Save RAID config to mdadm.conf file" }
mdadm --detail --scan --verbose >> /etc/mdadm.conf
```
