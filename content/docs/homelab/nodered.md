---
title: "nodeRED"
description: ""
summary: ""
date: 2024-09-13T00:15:18-07:00
lastmod: 2024-09-13T00:15:18-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

```Dockerfile
  nodered:
    container_name: nodered
    build:
      context: ./services/nodered/.
      args:
      - DOCKERHUB_TAG=latest
      - EXTRA_PACKAGES=
    restart: unless-stopped
    user: "0"
    environment:
    - TZ=${TZ:-Etc/UTC}
    ports:
    - "1880:1880"
    volumes:
    - ./volumes/nodered/data:/data
    - ./volumes/nodered/ssh:/root/.ssh
```
