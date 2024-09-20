---
title: "Grafana"
description: ""
summary: ""
date: 2024-09-20T00:26:04-07:00
lastmod: 2024-09-20T00:26:04-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Grafana Dashboards

### Templates

- `928` - Ubuntu Server dashboard

## Docker Compose

```Dockerfile
services:
  grafana:
    container_name: grafana
    image: grafana/grafana
    restart: unless-stopped
    user: "0"
    ports:
    - "3000:3000"
    environment:
    - TZ=Etc/UTC
    - GF_PATHS_DATA=/var/lib/grafana
    - GF_PATHS_LOGS=/var/log/grafana
    volumes:
    - ./volumes/grafana/data:/var/lib/grafana
    - ./volumes/grafana/log:/var/log/grafana
    healthcheck:
      test: ["CMD", "wget", "-O", "/dev/null", "http://localhost:3000"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
```
