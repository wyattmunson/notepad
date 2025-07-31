---
title: "Linux Configuration"
description: ""
summary: "Configuration of common Linux settings around network, static IP addresses, users, and groups."
date: 2024-06-24T15:29:47-07:00
lastmod: 2024-06-24T15:29:47-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

# Networking

## Set Static IP

A static IP is an IP address that doesn’t change. This allows a machine to set its own IP address, rather than be assigned a random one by the router. This means it will always be available at the same IP address, like `192.168.1.1`. This is contrasted with DHCP, where the IP is assigned randomly by the router.

### Network Manager

Network Manager is the more modern version of managing network configs like IP addresses. Network configurations are stored as YAML files and then applied using Network Manager.

#### Install NetworkManager

Network Manager needs to be installed from apt before using.

```bash {title="Installing network manager"}
# Install Network Manager with apt
sudo apt install network-manager -y
```

{{< callout context="note" title="File locations" icon="outline/info-circle" >}}
Netplan configuration YAMLs are stored in `/etc/netplan`.
{{< /callout >}}

The YAML file defines the network configs like IP address, gateway, and DNS servers.

```yaml {title="Network Manager YAML example"}
# scripts/network/01-static-ip.yaml
network:
  version: 2
  renderer: NetworkManager
  ethernets:
	eth0:
  	dhcp4: no
  	addresses:
    	- 192.168.1.42/24
  	gateway4: 192.168.1.1
  	nameservers:
    	addresses:
      	- 8.8.8.8
      	- 8.8.8.4
```

https://wiki.archlinux.org/title/NetworkManager

#### NetworkManager vs networkd

NetworkManager is typically used for desktops, while severs usually use networkd.

#### Test Netplan

Use `netplan test` to test a netplan configuration without apply the changes.

```bash {title="Test netplan configration without saving"}
sudo netplan try
```

#### Apply Netplan

Use `netplan apply` to apply the configuration. All configration files in `/etc/netplan` will be applied.

```bash {title="Apply netplan configration"}
sudo netplan apply
```

### Interface File

Updating the interfaces file is the "old" way of setting a static IP.

{{< callout context="note" title="File locations" icon="outline/info-circle" >}}
IP addresses are set in `/etc/network/interfaces`. DNS servers are set in `/etc/resolv.conf`.
{{< /callout >}}

```bash {title="Dyanmic IP address in /etc/network/interfaces"}
auto eth0
iface eth0 inet dhcp
```

```bash {title="Dyanmic IP address in /etc/network/interfaces"}
auto eth0
iface eth0 inet static
  address 192.168.0.100
  netmask 255.255.255.0
  gateway 192.168.0.1
  dns-nameservers 8.8.8.8
  dns-nameservers 8.8.4.4

```

```bash {title="DNS nameservers in /etc/resolv.conf"}
nameserver 208.67.222.222
nameserver 208.67.220.220
```

```json
{
  "title": "SIDECAR METADATA",
  "description": "Discover or synchrnoize...",
  "active": 0,
  "waiting": 0
}
```

```json
{
  "storage": {
    "used": 28.6,
    "total": 45
  },
  "server": {
    "status": "Online",
    "version": "v1.106.4"
  }
}
```

# Users

## Manage Users

```bash { title="Create and delete a user" }
# create user
sudo adduser USER_NAME

# delete user
sudo deluser USER_NAME

# set password
sudo passwd USER_NAME
```

{{< callout context="note" icon="outline/info-circle" >}}
Running `adduser` without arguments will create an interactive prompt for the password and full name.
{{< /callout >}}

```bash { title="Create user options" }
# create user
sudo adduser USER_NAME
sudo adduser --uid USER_ID USER_NAME
sudo adduser --system USER_NAME
```

{{< callout context="danger" title="Avoid providing password with -p flag" icon="outline/alert-octagon" >}}
When using the `-p` flag, the password should already be encrypted. The `passwd` command is the preferred option.
{{< /callout >}}

### User Location

| File Name     | Use                               |
| ------------- | --------------------------------- |
| `/etc/passwd` | Location of users                 |
| `/etc/shadow` | Location of hashed user passwords |

```bash { title="/etc/passwd file syntax" }
username:password:UID:GID:comment:home:shell
duke:x:1001:1001::/home/duke/:/bin/bash
```

Passwords were previously stored in `/etc/passwd`, making them viewable by everyone. The encrypted passwords are now stored in `/etc/shadow`.

## Manage User Groups

### Create a New Group

Create user group

```bash { title="Create new group" }
sudo groupadd GROUP_NAME
```

### Assign User to Group

Add user to group. User and group must already exist.

```bash { title="Add existing user to group" }
sudo adduser USER_NAME GROUP_NAME

# alternate
sudo useradd -G GROUP_NAME USER_NAME

# alternate
sudo usermod -a -G GROUP_NAME USER_NAME
```

Add user to multiple groups

```bash { title="Add user to multiple groups" }
sudo usermod -a -G GROUP_NAME_1,GROUP_NAME_2 USER_NAME
```

#### Create new user and assign to group

Create a new user and add to existing group. Group must already exist.

```bash { title="Create new user and assign to group" }
sudo useradd -G GROUP_NAME USER_NAME

# give new user a password
sudo passwd USER_NAME
```

#### Change users primary group

Change users primary group

```bash { title="Change user's primary group" }
sudo usermod -g GROUP_NAME USER_NAME
```

### Remove user from group

Remove user from group

```bash { title="Remove user from a group" }
sudo gpasswd -d USER_NAME GROUP_NAME
```

### View Groups

View all groups on the system with `cat /etc/groups`.

```bash { title="View all groups" }
sudo nano /etc/group
```

View the current user's groups with `groups`.

```bash { title="View current user's groups" }
groups
```

View a given user's groups with `groups USER_NAME`.

```bash { title="View a given user's groups" }
groups USER_NAME
```

View a given user's Id and group Ids with `id USER_NAME`.

```bash { title="View a given user's Id and group Ids" }
id USER_NAME
```

View all users and their group associations with `getent group`.

```bash { title="View what users are members of which groups" }
getent group
```

### Common Groups

| Group     | Allows users to...                         |
| --------- | ------------------------------------------ |
| `sudo`    | Use the sudo command to elevate prilieges. |
| `wheel`   | An older way to endow sudo privlieges..    |
| `cdrom`   | Mount the optical drive                    |
| `adm`     | Monitor linux system logs                  |
| `lpadmin` | Configure printers                         |
| `plugdev` | Access external storage devices            |

## Manage File Ownership

```bash
# change owner to root
chown root file.txt
# change group to admin
chown :admin file.txt
# change owner to root, and group to admin
chown root:admin file.txt

# recursive (affects all subdirectories)
chown -R root:admin ./some/dir
```

## Bash Config Files

Bash config files set common variables or functions that are run or set everytime a new login shell is invoked (a new terminal tab is opened). This can set frequently used variables or aliases, format the appearance of a shell, and more.

When invoking a terminal shell, there are two types of shells:

1. **Login/Interactive shell**: This is when someone logs into the terminal and is typing commands into a terminal shell. If a command requires additional user prompts, the user can type them in.
1. **Non-interactive shell**: This is when a bash shell runs in the background or in a CI/CD pipeline. If a command requires additional prompts,

Bash config files are typically located in the user's home directory (`~`).

### Login shell order

Bash checks for configuration files in the following order:

```bash { title="Config file order" }
# config file order, starting at top:
~/.bash_profile
~/.bash_login
~/.profile
```

{{< callout context="caution" icon="outline/alert-triangle" >}}
Bash uses the first, and only the first, configuration file it finds. Subdequent file are ignored.
{{< /callout >}}

#### .bash_profile

| File Name       | When Sourced                                | Typical Content                                              |
| --------------- | ------------------------------------------- | ------------------------------------------------------------ |
| `.bash_profile` | Once during an interactive login shell      | Environment variables for login shell, startup scripts       |
| `.bash_login`   | Fallback if .bash_profile is missing        | Same as .bash_profile                                        |
| `.profile`      | All interactive shells (login & sub-shells) | Non-login specific environment variables, aliases, functions |

- Keep `.bash_profile` lean

### System Wide Login Config

While bash config files are typically located in a user's home directory, there are configs that are loaded for all users.

{{< callout note >}} The system-wide bash config file is stored at `/etc/profile`. {{< /callout >}}

The system-wide config for Ubuntu is located at`/etc/profile`. This can be used to give a login message for all users.
