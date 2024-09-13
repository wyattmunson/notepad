---
title: "External Drives"
description: ""
summary: ""
date: 2024-09-12T21:40:15-07:00
lastmod: 2024-09-12T21:40:15-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Mount External Drive

### Find external drive and partition

First, use `fdisk` to find the external drive.

```bash { title="Find external drive" }
$ sudo fdisk -l
```

This will show the attached devices, as well as the partitions.

```bash { title="Interpret fdisk output" }
$ sudo fdisk -l

# OUTPUT:
Disk /dev/sdf: 7.28 TiB, 8001563222016 bytes, 15628053168 sectors
Disk model: 004-3CP202
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: E97B7CD64-9712-49F0-8183-2B07B703DC4E

Device      Start         End     Sectors  Size Type
/dev/sdf1      40      409639      409600  200M EFI System
/dev/sdf2  409640 15627790983 15627381344  7.3T Apple HFS/HFS+
```

In the above example, `/dev/sdf` is the attached hard drive, but `/dev/sdf2` is the partition or device we're after.

### Create mount point

Create the directory for the mount point.

```bash { title="Create mount directory" }
mkdir /mnt/wydrive
```

### Mount external drive

Use the mount command to

```bash { title="Mount external drive" }
mount /dev/sdf2 /mnt/wydrive
```

### Read Apple filesystems

Install `hfsprogs` to read Apple filesystems.

## Create RAID5 Array

### Install mdadm Utility

Install mdadm

```bash { title="Install mdadm" }
sudo apt update
sudo apt install mdadm -y
```

### Create RAID 5 Array with mdadm

```bash { title="Create RAID 5 array and specify devices" }
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb1 /dev/sdc1 /dev/sdd1
```

This process can take time. A RAID 5 array with three 12 TB drives was estimated to take 15 hours.

### Monitor creation of RAID array

Monitor creation of RAID devices

```bash { title="Monitor creation of RAID devices" }
cat /proc/mdstat
```

Use watch command to keep the command running. Refreshes every 2 seconds.

```bash { title="Monitor creation of RAID devices" }
watch cat /proc/mdstat
```

### Inspect each device

Inspect status of each device (drive) with mdadm.

```bash { title="Inspect status of each device" }
mdadm -E /dev/sdb1
```

### Inspect RAID 5 device

Inspect status of each device (drive) with mdadm.

```bash { title="Inspect status of each device" }
mdadm --detail /dev/md0
```

### Create Filesystem

Using the /dev/md0 device, create an ext4 filesystem.

Create Filesystem with ext4 format
Use mkfs.ext4 to create the filesystem.

```bash { title="Use mkfs to create filesystem" }
sudo mkfs.ext4 /dev/md0
```

### Verify creation of filesystem with lsblk command.

```bash { title="Verify filesystem with lsblk" }
lsblk
```

### Mount newly created device

Use mount command to mount the device.

```bash { title="Mount the device" }
sudo mount /dev/md0 /mnt/wysrive
```

Verify the device is mounted with df.

```bash { title="Use df to verify filesystem is created" }
$ df -h
/dev/md0                        	22T   24K   21T   1% /mnt
```

### Persist device across reboots

Mount the device using the mount command.

```bash { title="Get fstab" }
$ cat /etc/fstab
/dev/md0            	/mnt 	ext4   defaults            	0 0
```

By default, there is no RAID configuration. On reboot, the RAID configuration is not guaranteed to be in md0 rather something else.

Save RAID configuration to file

```bash { title="Save mdadm.conf config for reboots" }
mdadm --detail --scan --verbose >> /etc/mdadm.conf
```

Was unable to run this command directly. Had to capture the output of the mdadm command and write to the /etc/mdadm.conf file directly.

sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf

Automatic Mounting at Boot
echo '/dev/md0 /mnt/md0 ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab
