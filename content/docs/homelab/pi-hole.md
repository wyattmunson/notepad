---
title: "Pi-Hole"
description: ""
summary: "Install and Manage Pi-Hole ad blocker and internal DNS service."
date: 2025-01-05T23:59:33-08:00
lastmod: 2025-01-05T23:59:33-08:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Install

### Docker Compose

```yaml { title="docker-compose.yml" }
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
      - "6700:80/tcp" # Custom external port of 6700
    environment:
      TZ: "America/New_York"
      WEBPASSWORD: "SOMEsecretPASSWORD"
    volumes:
      - "./etc-pihole:/etc/pihole"
      - "./etc-dnsmasq.d:/etc/dnsmasq.d"
    cap_add:
      - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
    restart: unless-stopped
```

### Port Binding Issue

If the container fails to start because of a port conflict on 53, `systemd-resolved` is the likely culprit.

#### Stop systemd-resolved Service

```bash { title="Stop systemd-resolved service" }
sudo systemctl stop systemd-resolved.service
```

#### Update resolved.conf File

Edit `/etc/systemd/resolved.conf file, and uncomment line with DNSStubListener and change yes to no.

```bash { title="Use pip to install requirements.txt file" }
Update /etc/systemd/resolved.conf file
# old value
#DNSStubListener=yes


# new value
DNSStubListener=no
```

#### Restart systemd-resolved Service

Restart service after editing `resolve.d` file.

```bash { title="Restart systemd-resolved service" }
sudo systemctl restart systemd-resolved.service
```

#### Fix local DNS Resolution

Remove symlink to the systemd stub-resov.conf and replace it with a link to a full resolv.conf.

```bash { title="Restart systemd-resolved service" }
sudo rm /etc/resolv.conf
sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

## Web UI

{{< callout context="note" title="Access admin panel" icon="outline/code" >}}
Add `/admin` to the IP of the machine to access the admin panel. Syntax is `IP_ADDRESS:6700/admin` if using the same port in the docker-compose.yml file above.
{{< /callout >}}

Access at `<local_ip>/admin`, if using the default port of `80`. The docker-compose.yml above updates the external port to `6700`.

## Internal UI

Local DNS records allow for internal routing when inside the home network.

The DNS Records page allows input of domain names and an IP to resolve to. The DNS record does not need a TLD meaning a single word would create a DNS record.

Use `*.home.arpa` to create a valid and safe domain name. Records would be `fileserver.home.arpa`. The Pi-Hole internal DNS does not allow for wildcards.
