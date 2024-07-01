---
title: "Homelab"
description: ""
summary: ""
date: 2024-07-01T04:06:52-07:00
lastmod: 2024-07-01T04:06:52-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

bash <(curl -s https://gitlab.com/NickBusey/HomelabOS/-/raw/master/install_homelabos.sh)

```bash
========== Preparing HomelabOS docker image ==========
========== This account is NOT in the docker group ==========
Do you want to add yourself to the docker group? [y/n] y
docker:x:997:
========== You must log out and back in for changes to take effect. ==========
========== You may need to reboot entirely if you still get permission denied. ==========
make: *** [Makefile:38: build] Error 1

You can check the status of Organizr with 'systemctl status organizr' or 'sudo docker ps'
To enable more services, run 'cd /var/homelabos/install' then 'make set servicename.enable true'
where servicename is a service you would like to have.

Example: 'make set miniflux.enable true'

Once you have enabled all the services you would like, simply run 'make deploy'.

================== Done.  ==================
```
