---
title: "IOTStack"
description: ""
summary: "Install and manage IOTStack: a installation and management wrapper around IoT-related Docker containers."
date: 2024-09-13T00:17:59-07:00
lastmod: 2024-09-13T00:17:59-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Backup IOTStack

# generate backup, with timestamp, with specified user

```bash { title="Run backup script" }
./scripts/backup.sh 1 USER_NAME
```

Backup types:

- `1` - Backup name with timestamp
- `2` - Backup name with day of week (0-6)
- `3` - Both types

Backup output location: ./backups/backup/backup_2024-07-07_0218.tar.gz

Backup tarball location

```bash { title="Backup tarball location" }
./backups/backup/backup_DATE_0000.tar.gz
```
