---
title: "Telegraf"
description: "Use Telegraf"
summary: "Telegraf makes infrastructure monitoring simple"
date: 2024-07-05T15:16:23-07:00
lastmod: 2024-07-05T15:16:23-07:00
draft: false
weight: 50
categories: ["guides"]
tags: ["telegraf", "observability"]
contributors: ["Wyatt"]
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Telegraf makes monitoring your infrastrcture drop dead simple. With a few configuration commands, this can be added.

### Inputs

Telegraf inputs are data sources, like information from a linux system, kubernetes cluster, docker container, ect.

Outputs are where Telegraf sends that data to. We will use Influx as the database.

## Setup Influx DB

Telegraf needs some sort of database to send data to, and we'll use Influx DB. The Infflux db needs to be created before Telegraf is configured.

```yaml { title="docker-compose.yml" }
version: "3.6"

networks:
  default:
    driver: bridge
    ipam:
      driver: default

services:
  influxdb:
    container_name: influxdb
    image: "influxdb:1.8"
    restart: unless-stopped
    ports:
      - "8086:8086"
    environment:
      - TZ=Etc/UTC
      - INFLUXDB_HTTP_FLUX_ENABLED=false
      - INFLUXDB_REPORTING_DISABLED=false
      - INFLUXDB_HTTP_AUTH_ENABLED=false
      - INFLUXDB_MONITOR_STORE_ENABLED=FALSE
    # - INFLUX_USERNAME=user
    # - INFLUX_PASSWORD=pass
    # - INFLUXDB_UDP_ENABLED=false
    # - INFLUXDB_UDP_BIND_ADDRESS=0.0.0.0:8086
    # - INFLUXDB_UDP_DATABASE=udp
    volumes:
      - ./volumes/influxdb/data:/var/lib/influxdb
      - ./backups/influxdb/db:/var/lib/influxdb/backup
    healthcheck:
      test: ["CMD", "curl", "http://localhost:8086"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
```

```bash { title="Start docker-compose stack" }
docker-compose up -d
```

## Install Telegraf

```bash { title="Install Telegraf on Ubuntu" }
# enable Telegraf to start on boot
sudo systemctl enable telegraf
```
