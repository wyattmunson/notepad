---
title: "Mosquitto"
description: ""
summary: ""
date: 2024-09-13T00:16:29-07:00
lastmod: 2024-09-13T00:16:29-07:00
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

```Dockerfile
  mosquitto:
    container_name: mosquitto
    build:
      context: ./.templates/mosquitto/.
      args:
      - MOSQUITTO_BASE=eclipse-mosquitto:latest
    restart: unless-stopped
    environment:
    - TZ=${TZ:-Etc/UTC}
    ports:
    - "1883:1883"
    volumes:
    - ./volumes/mosquitto/config:/mosquitto/config
    - ./volumes/mosquitto/data:/mosquitto/data
    - ./volumes/mosquitto/log:/mosquitto/log
    - ./volumes/mosquitto/pwfile:/mosquitto/pwfile
```
