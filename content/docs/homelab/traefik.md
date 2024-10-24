---
title: "Traefik"
description: ""
summary: "Install and manage Traefik: a webserver and reverse proxy."
date: 2024-07-29T21:11:20-07:00
lastmod: 2024-07-29T21:11:20-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Route to External Services

Route to external services by create a yaml config in the `conf.d` folder.

{{< callout context="note" icon="outline/alert-triangle" >}}
Create yaml file in `/var/homelabos/traefik/conf.d`
{{< /callout >}}

```yaml
http:
  routers:
    authentik-http:
      rule: "Host(`authentik.subdomain.example.com`)"
      entryPoints:
        - "http"
      middlewares:
        - "customFrameHomelab@file"
      service: "authentik"
    authentik:
      rule: "Host(`authentik.subdomain.example.com`)"
      entryPoints:
        - "https"
      middlewares:
        - "customFrameHomelab@file"
      service: "authentik"
      tls:
        certResolver: "http"

  services:
    authentik:
      loadBalancer:
        passHostHeader: true
        servers:
          - url: "http://192.168.1.100:9000"
```
