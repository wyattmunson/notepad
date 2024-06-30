---
title: "Telegraf"
description: ""
summary: ""
date: 2024-06-24T19:54:06-07:00
lastmod: 2024-06-24T19:54:06-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Use Telegraf as the agent to collect system logs and metrics.

## Setup Telegraf Monitoring

Currently Telegraf was setup in the IOTstack. This runs inside the docker compose network, and collects metrics about what’s happening in the network.

- Create Influx DB
- Install Telegraf
- Write configuration file
- Start and enable with systemctl

### Create Influxdb Databases

Create a database in Influx db that Telegraf will post to. The database should be created before instantiating Telegraf or there’ll be nowhere to send the data to.

```bash { title="Interact with Influxdb CLI" }
# exec into influx container
sudo docker exec -it influxdb influx

# create new database
create database machine_metrics
# use database machine_metrics
use machine_metrics
# show all databases
show databases
show series
show measurements	# like tables
show tag keys		  # show tag keys for each measurement
show field keys		# like column names and datatypes for each measurement
```

### Install Telegraf

Telegraf can be installed in a Docker container or as an apt package and managed through systemd.

```bash { title="Install telegraf from InfluxData apt repo" }
curl -s https://repos.influxdata.com/influxdata-archive.key > influxdata-archive.key
echo '943666881a1b8d9b849b74caebf02d3465d6beb716510d86a39f6c8e8dac7515 influxdata-archive.key' | sha256sum -c && cat influxdata-archive.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list
sudo apt-get update && sudo apt-get install telegraf
```

### Configure Telegraf

#### Default Location

{{< callout context="note" icon="outline/info-circle" >}}
Telegraf config sits in `/etc/telegraf/telegraf.conf`
{{< /callout >}}

The `/etc/telegraf/telegraf.conf` file drives what metrics Telegraf selects and any filters or conditions. Restart Telegraf when updating this file for changes to take effect.

#### List of all Telegraf inputs

List of Telegraf input plugins: [GitHub](https://github.com/influxdata/telegraf/tree/master/plugins/inputs)

The full Telegraf reference can be found in `/etc/telegraf-reference.conf`. It is a full list of inputs and configurations for reference only. The file is read only and changes to it are overridden.

**Generate default config file**:

```bash
telegraf config > telegraf.conf
```

{{< callout context="caution" icon="outline/info-circle" >}}
The Telegraf systemd service needs to be restarted after `telegraf.conf` is updated.

```bash
sudo systemctl restart telegraf
```

{{< /callout >}}

```toml { title="Example /etc/telegraf/telegraf.conf"}
[global_tags]
[agent]
  interval = "10s"
  round_interval = true
  metric_batch_size = 1000
  metric_buffer_limit = 10000
  collection_jitter = "0s"
  flush_interval = "10s"
  flush_jitter = "0s"
  precision = "0s"
  hostname = ""
  omit_hostname = false
[[inputs.cpu]]
  percpu = true
  totalcpu = true
  collect_cpu_time = false
  report_active = false
  core_tags = false
[[inputs.disk]]
  ignore_fs = ["tmpfs", "devtmpfs", "devfs", "iso9660", "overlay", "aufs", "squashfs"]
[[inputs.diskio]]
[[inputs.kernel]]
[[inputs.mem]]
[[inputs.processes]]
  use_sudo = false
[[inputs.swap]]
[[inputs.system]]
[[inputs.file]]
  files = ["/sys/class/thermal/thermal_zone0/temp"]
  name_override = "cpu_temperature"
  data_format = "value"
  data_type = "integer"
[[inputs.net]]
[[outputs.influxdb]]
  urls = ["http://localhost:8086"]
  database = "machine_metrics"
  timeout = "5s"

# added to get TCP connections
[[inputs.netstat]]

# added for kubernetes
[[inputs.kubernetes]]
  url="http://192.168.1.100:10250"
  insecure_skip_verify = true
  bearer_token_string = "XXXXXXXXXXXXXXXXXXX"
```
