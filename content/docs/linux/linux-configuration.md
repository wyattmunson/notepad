---
title: "Linux Configuration"
description: ""
summary: ""
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

Network Manager needs to be installed from apt before using.

```bash {title="Installing network manager"}
# Install Network Manager with apt
sudo apt install network-manager -y
```

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
