---
title: "Home Assistant"
description: "Test Description"
summary: "Install and manage Home Assistant: a single tool to maange a myriad of IoT platforms."
date: 2024-09-20T01:12:20-07:00
lastmod: 2024-09-20T01:12:20-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Docker Compose

```Dockerfile
services:
  home_assistant:
    container_name: home_assistant
    image: ghcr.io/home-assistant/home-assistant:stable
    restart: unless-stopped
    network_mode: host
    volumes:
    - /etc/localtime:/etc/localtime:ro
    - ./volumes/home_assistant:/config
    privileged: true
```
