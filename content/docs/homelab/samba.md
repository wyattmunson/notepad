---
title: "Samba"
description: ""
summary: "Install and manage Samba: a linux-based fileserver."
date: 2024-09-12T22:11:15-07:00
lastmod: 2024-09-12T22:11:15-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Samba is a fileserver for Linux.

## Install Samba

```bash { title="Install Samba" }
sudo apt install samba -y
```

Validate install

```bash { title="Validate install" }
whereis samba
```

Confirm Sambda is running

```bash { title="Validate Samba is running" }
systemctl status smbd
```

### Create share directory

```bash { title="Create share directory" }
mkdir /mnt/md0/share
```

### Create Samba conf file

Edit Sambda conf file

```bash { title="Edit Samba conf file" }
sudo vim /etc/samba/smb.conf
```

Validate syntax of conf file

```bash { title="Validate Samba conf file" }
testparm
```

### Add User to Samba

```bash { title="Add user" }
# The user must exist on the system
sudo smbpasswd -a USER_NAME
```

### Update Conf File

```
[sharing]
	comment = Samba share directory
	path = /mnt/md0/share
	read only = No
	valid users = @mainframe
```

### Open firewall ports (if necessary)

Allow Samba

```bash { title="Allow Samba in firewall" }
sudo ufw allow samba
```

### Restart samba

Required for Samba config changes to take effect.

```bash { title="Restart Samba" }
sudo systemctl restart smbd
```

ISSUE
On restart did not create the filesystem at md0 but md127
This was rectified by running sudo mount /dev/md127 /mnt/md0 but it should be able to mount the correct device or name the filesystem properly
When running df -h it did not appear. Can be seen with lsblk command

## Samba Client

After creating the Samba server, clients are established to connect to the server.

### Install Samba Client packages

```bash { title="Install Samba Client packages" }
sudo apt install cifs-utils psmisc
```

Verify LinuxCIFS is available (no output is good)

```bash { title="Verify LinuxCIFS" }
mount -t cifs
```

Validate fuser

```bash { title="Validate fuser" }
fuser
```

Create samda mount point directory

```bash { title="Create mount point" }
sudo mkdir /mnt/homeblock
```

Mount Samba directory

```bash { title="Mount Samba directory" }
mount -t cifs //192.0.2.17/SharedFiles /mnt/smb_share
mount -t cifs //192.168.1.115 /mnt/homeblock
mount -t cifs //[server-ip]/[share-path] /[mount-point]
```

Actual mount command used:
sudo mount -t cifs //192.168.0.45/hb /mnt/hb -o username=UUUUUUUUU,password=XXXXX
