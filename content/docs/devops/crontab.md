---
title: "Crontab"
description: ""
summary: ""
date: 2024-07-30T00:07:23-07:00
lastmod: 2024-07-30T00:07:23-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## Cron Basic

### Cron Notes

- Times for crontab are in UTC
- There can be a 60 second delay before a new crontab is picked up by crond

## Cron Syntax

`* * * * * * /bin/sh /path/to/script.sh`

`06 0 * * 2,4 /home/mainframe/server-tools/venv/bin/python /home/mainframe/server-tools/s3syncd.py /home/mainframe/ theflame/home-backup`

## Cron Location

System-wide cron confguration is stored in `/etc/crontab`, which is automatically generated from these files: `/etc/cron.hourly`, `/etc/cron.daily`, `/etc/cron.weekly`, and `/etc/cron.monthly`.

- User's cron tab `crontab -l`
- Root user `sudo crontab -e`
- In /etc/cron.\*
- System crontab file `/etc/crontab`
-

### Install crontab for current user

```bash
crontab -e
```

```bash
# edit current user's crontab
crontab -e

# view current user crontab
crontab -l

```

## Cron Logs

In Ubuntu, cron logs are dumped in `/var/log/syslog`.

```bash { title="Use grep to find cron logs" }
grep CRON /var/log/syslog
```
