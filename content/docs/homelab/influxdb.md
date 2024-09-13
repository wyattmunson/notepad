---
title: "InfluxDB"
description: ""
summary: ""
date: 2024-09-13T00:16:20-07:00
lastmod: 2024-09-13T00:16:20-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Install InfluxDB

### Dockerfile

```Dockerfile
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
  # - INFLUX_USERNAME=dba
  # - INFLUX_PASSWORD=supremo
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

## Using InfluxDB

### Example Insert Query

Below is an insert query. There is no explicit command to create a table. Below, api_logs is the measurement inside the n8n_api_metrics database.

```bash { title="Create Influx DB (example)" }
INSERT INTO api_logs(endpoint, ip_address, request_body, success)
VALUES ('/api/v1/users', '192.168.0.1', '{"user":"example"}', 1);
```

Actual Query

```bash { title="Actual insert query" }
INSERT INTO api_logs(endpoint, ip_address, request_body, status, direction)
VALUES ('/api/v1/users', '192.168.0.1', '{"user":"example"}', 'started', 'inbound http');
```

Create Influx DB (example)

Using InfluxDB
Below is an insert query. There is no explicit command to create a table. Below, api_logs is the measurement inside the n8n_api_metrics database.
Basic InfluxDB usage

```bash { title="Basic InfluxDB usage" }
# exec into influx container
docker exec -it a0d influx

# create a database
create database n8n_api_metrics;

# show databases
show databases

# use database
use n8n_api_metrics
```
