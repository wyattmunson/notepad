---
title: "Airprint"
description: ""
summary: ""
date: 2024-10-30T00:37:11-07:00
lastmod: 2024-10-30T00:37:11-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Install CUPS

Update and install CUPS and Avahi:

```bash
sudo apt update
sudo apt install cups avahi-daemon
```

Add your user to the lpadmin group to manage printers:

```bash
sudo usermod -aG lpadmin $USER
```
